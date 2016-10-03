Backups/Network Storage
=======

Local Backups
--------------

The `docker-compose` environments on a.mobilizingcs.org are backed up nightly to stor.mobilizingcs.org. This is handled via a simple bash script that invokes a number of one-off [docker-backup](https://hub.docker.com/r/boombatower/docker-backup/) containers, mounting the volumes from each docker-compose container we'd like to back up and tarring the contents.

The current version of this script can be found at `a.mobilizingcs.org:/root/bin/backup.sh`, but here's a basic copy:

```sh
#!/bin/bash

# array of containers to back up
containers=(container_1 container_2 container_3)

for i in "${containers[@]}"
do
  # dump backups to storage box. overwriting.
  docker run --rm --volumes-from $i -v /mnt/backup:/backup boombatower/docker-backup backup "$i.tar.xz"
done
```

S3 Backups
-----------

Once a week, the tarred backups found in "Local Backups" are replicated to s3 using a [simple docker container](https://github.com/stevenolen/encrypt-and-s3-docker). This container requires that you pass AWS creds as well as mount the volume you'd like encrypted/replicated at `/backup` and an openssl public key mounted at `/key.pub`. The container will, for each file (that match a given regex, default: `*`) within the `/backup` directory:
  * generate a random password to use for symmetric encryption of the file
  * encrypt the file using the generated random password
  * encrypt the random password using `/key.pub`
  * upload the encrypted file to s3 with added `.enc` extension
  * upload the encrypted random password to s3 next to the file as `.key.enc`
  * remove encrypted files.

Take a look at `a.mobilizingcs.org:/root/bin/backup-s3.sh` for any other tweaks.

ZFS datasets on stor.mobilizingcs.org
--------------------------------------

stor.mobilizingcs.org runs FreeBSD 10, and makes use of ZFS for data storage and integrity. 

```sh
[root@stor ~]# zfs list
NAME           USED  AVAIL  REFER  MOUNTPOINT
rpool         6.68G  47.1G    96K  /rpool
rpool/root    1.82G  47.1G  1.82G  /
rpool/tmp       96K  47.1G    96K  /tmp
rpool/usr     2.93G  47.1G  2.93G  /usr
rpool/var     1.90G  47.1G  1.90G  /var
tank          1.61T  10.7T    21K  /tank
tank/archive  1.50T  10.7T  1.50T  /tank/archive
tank/backup   3.21G  10.7T   411M  /export/backup
tank/home      114G  10.7T  50.7G  /export/home
```

### tank/backup
Mounted to `a.mobilizingcs.org` for production docker container backups. Makes use of `sanoid` (see `/etc/sanoid/sandoid.conf`) to perform auto snapshots to store data weekly.

### tank/home
A legacy mount of user home directories used during the heavier usage times of the grant.

### tank/archive
Contains a very large amount of archival data, some backup exports from former production servers (which general can just be untarred and used as needed), as well as proxmox vma backups (see proxmox section below).  Additionally files `lausd[2013..2016].sql.gz` are database dump from our production server through the years. These can be restored to check out data changing over time if necessary.

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
