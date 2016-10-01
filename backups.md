Backups/Network Storage
=======

Restore from Proxmox Legacy `.vma` backups
-------------------------------------------

Proxmox (our historical kvm solution) has a proprietary archive format for their virtual machine backups.  While our legacy vm data should exist on our backup server or on s3, there may come a time when the legacy archives are need (to reference conf files, or "how we did things way back when."). The process to restore a backup is a bit lengthy, but here's how it goes.

  * Copy the `vzdump-qemu-*.vma  file you need from `/tank/archive/vms` on `stor` to a docker host.
  * Run a container mounting the directory with the `.vma` to `/images`: docker run --rm -t -i -v /path/to/vma/:/images akaihola/vma /bin/bash
  * Extract the vma in the container: `vma extract vzdump-qemu-*.vmz`
  * outside of the container, you'll need to mount the raw disks, have their vgs picked up and activated:
    * `losetup /dev/loop0 disk-drive-virtio0.raw` (for every raw disk, enumerating `loopX`)
    * `kpartx -a /dev/loop0` (for every raw disk, enumerating `loopX`)
    * `vgscan` and then activate the newly found vg (`lausd` in this example): `vgchange -ay lausd`
    * mount the lg you need: `mount /dev/mapper/lausd-root /mnt/loop`
    * use the fs as needed at `/mnt/loop`
    * when you're done, unmount: `umount /mnt/loop`, deactivate: `vgchange -an lausd`, and delete the partitions: `kpartx -d /dev/loop0` (for each `loopX` created above)
    * `vgscan` and `df` to confirm they are gone and then delete the `.raw` files to clean up!


Below is legacy, legacy and more legacy


Setup
----------

Much of this process is composed with the [cens-backup](https://github.com/mobilizingcs-ops/chef-cens-backup) chef resources.

Hardware
----------

Two hardware pieces are used for this process: `starbuck` and `cavil`.

  * `starbuck`
    * FreeBSD 10.1
    * 4U storage box with zfs pool of 7 mirrors (approx usable storage is 12.5TB)
    * Basic zfs configuration [here](https://github.com/mobilizingcs-ops/chef-cens-backup/blob/master/recipes/server.rb)
  * `cavil`
    * FreeBSD 10.1
    * 2U legacy Dell box with 4x2TB drives as a pool of 2 mirrors (~4TB usable).
    * Basic zfs configuration [here](https://github.com/mobilizingcs-ops/chef-cens-backup/blob/master/recipes/server-dup.rb)

The important data on `starbuck` is synced once a day to `cavil` via syncoid and using zfs send/recv.

App Data Backups
-----------------

The [ruby backup gem](http://meskyanichi.github.io/backup/v4/) is used to model/execute backups on our servers.  Currently there are models for `ohmage`, `rt4`, `generic_webserver`, `jenkins`, `ldap` and `rstudio` (see their [recipes](https://github.com/mobilizingcs-ops/chef-cens-backup/tree/master/recipes) for definitions).  The backups are made nightly and data is stored on `starbuck` at tank/backups under the host's directory. Generally speaking, these are overwritten nightly and zfs snapshotting is used heavily on `starbuck` to store this data for a longer period of time.

Offsite Policy
--------

After the [starbuck incident](starbuck_oops.jpg) in 2015, we got a lot more serious about offsite backups! Based on this, we now store the important data from our network encrypted on S3. 

  * ohmage_aws
    * this handles taking an ohmage backup model, encrypting it, and sending the artifact to S3.
    * executed nightly.
  * [zfs_datasets](https://github.com/mobilizingcs-ops/chef-cens-backup/blob/master/recipes/zfs_datasets.rb)
    * this handles tarring the zfs datasets of important data and moving to S3.
    * this is executed on `cavil` so as not to add unnecessary load to `starbuck`
    * executed weekly. 