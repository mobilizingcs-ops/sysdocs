Chef
=======

I wont even try to claim that I can teach someone how to fully use chef or system automation tools in a wiki page, but this page serves to give someone familiar with chef a headstart in exactly how we are using this tool.

Cookbooks
----------

Since I started learning/doing chef in late 2014, most/all of the cookbook interaction on our chef server is done via [wrapper cookbooks](https://www.chef.io/blog/2013/12/03/doing-wrapper-cookbooks-right/). Community cookbooks are used to interface with package creation/configuration/management, and wrapped inside simple `cens-` cookbooks where defaults are overwritten/managed and community recipes are chained together to actually create our infrastructure as it exists. Below is a list of the cookbooks currently in use and a brief description of how they are being used

  * [cens-base](https://github.com/mobilizingcs-ops/chef-cens-base)
    * Sets up some basics on all nodes: installs `emacs`, `vim`, `curl`, `openssh`, `ntp`, `htop`, `nfs-client`, `chef-client service`, configures these and sets the timezone to `US/Pacific`.
  * [cens-monitor](https://github.com/mobilizingcs-ops/chef-cens-monitor)
    * `[master, master-graphite, master-grafana]` recipes configure a server to act as the sensu server, as well as collecting/visualizing metric data sent from sensu metric checks by the clients
    * The additional recipes configure/set up checks on clients: `base_checks` can be used on all nodes, `ohmage_checks` on those running ohmage, `raid_checks` on physical nodes with raid arrays, `mysql_checks` on servers running mysql, etc.
  * [cens-backup](https://github.com/mobilizingcs-ops/chef-cens-backup)
    * `[server, server-dup]` recipes set up the ZFS storage servers NFS shares needed for servers to back up to, and a nightly sync of important data from the main server to the dup.
    * Additional recipes configure/set up backup models (see [ruby backup gem](http://meskyanichi.github.io/backup/v4/)) for running services: `[ohmage, rstudio, jenkins, generic web server, request-tracker4]`
  * [cens-dhcp](https://github.com/mobilizingcs-ops/chef-cens-dhcp)
    * Configures a server to handle dhcp requests for the CENS domain.
  * [cens-unifi](https://github.com/mobilizingcs-ops/chef-cens-unifi)
    * Configures a server to act as the server for the Unifi AP in the Lab.

Run Lists
----------

Chef operates by storing a `run_list` for each node it manages.  Each run converges the node on a state where all recipes in the list are completed. To keep this simple, here's an output from `knife` on 3/4, 10:15pm which shows the items in each node's run_list (you can also use this as a reference for how to generate a more recent list using `knife`):

```
Steves-MacBook-Pro$ knife search '*.*' -a run_list
24 items found

edu.ohmage.org:
  run_list: recipe[cens-base], recipe[monitor::base_checks]

support.mobilizingcs.org:
  run_list: recipe[cens-base], recipe[monitor::base_checks], recipe[cens-backup::rt4]

alerts.ohmage.org:
  run_list: recipe[cens-base], recipe[monitor::master], recipe[monitor::redis], recipe[monitor], recipe[monitor::base_checks], recipe[monitor::_mailer_handler], recipe[monitor::_graphite_handler], recipe[monitor::purge_check_mk], recipe[monitor::master-grafana]

starbuck.ohmage.org:
  run_list: recipe[cens-base], recipe[cens-backup::server]

athena.ohmage.org:
  run_list: recipe[cens-base], recipe[monitor::base_checks], recipe[monitor::purge_check_mk]

dev.opencpu.org:
  run_list: recipe[cens-base], recipe[monitor::base_checks], recipe[monitor::purge_check_mk]

apollo.ohmage.org:
  run_list: recipe[cens-base], recipe[monitor::base_checks], recipe[monitor::purge_check_mk]

lausd.mobilizingcs.org:
  run_list: recipe[cens-base], recipe[monitor::base_checks], recipe[monitor::ohmage_checks], recipe[monitor::mysql]

rstudio.mobilizingcs.org:
  run_list: recipe[cens-base], recipe[monitor::base_checks], recipe[cens-backup::rstudio]

ci.ohmage.org:
  run_list: recipe[cens-base], recipe[monitor::base_checks], recipe[cens-backup::jenkins]

cavil.ohmage.org:
  run_list: recipe[cens-base], recipe[cens-backup::server-dup]

anvil.ohmage.org:
  run_list: recipe[cens-base], recipe[monitor::base_checks], recipe[monitor::purge_check_mk]

dev1.opencpu.org:
  run_list: recipe[cens-base], recipe[monitor::base_checks], recipe[monitor::purge_check_mk]

dev2.opencpu.org:
  run_list: recipe[cens-base], recipe[monitor::base_checks], recipe[monitor::purge_check_mk]

analysis02.ohmage.org:
  run_list: recipe[cens-base], recipe[monitor::base_checks], recipe[monitor::purge_check_mk]

ocpu.ohmage.org:
  run_list: recipe[cens-base], recipe[opencpu], recipe[opencpu::cache], recipe[monitor::base_checks]

ldap.ohmage.org:
  run_list: recipe[cens-base], recipe[monitor::base_checks], recipe[monitor::purge_check_mk], recipe[cens-backup], recipe[cens-backup::ldap]

doral.ohmage.org:
  run_list: recipe[cens-base], recipe[monitor::base_checks], recipe[monitor::raid_checks]

boomer.ohmage.org:
  run_list: recipe[cens-base], recipe[monitor::base_checks], recipe[monitor::raid_checks]

cens.ohmage.org:
  run_list: recipe[cens-base], recipe[cens-dhcp], recipe[cens-unifi], recipe[monitor::base_checks]

pilots.mobilizelabs.org:
  run_list: recipe[cens-base], recipe[monitor::base_checks], recipe[cens-backup::ohmage], recipe[monitor::ohmage_checks]

test.mobilizingcs.org:
  run_list: recipe[cens-base], recipe[monitor::base_checks], recipe[monitor::purge_check_mk], recipe[monitor::mysql], recipe[monitor::ohmage_checks], recipe[cens-backup::ohmage]

eight.ohmage.org:
  run_list: recipe[cens-base], recipe[monitor::base_checks], recipe[monitor::purge_check_mk]

web.ohmage.org:
  run_list: recipe[cens-base], recipe[monitor::base_checks], recipe[cens-backup::generic_webserver]
```

Converge a Node
----------------

This isn't quite as simple as it'll sound, but the basics are here.  If you'd like to converge a blank node (eg. a running server with no particular packages installed), you'll follow these steps:

  * Bootstrap the node using knife (make sure you have ssh access)
    * `knife bootstrap #{new_node_ip} -X #{username} -r cens-base` you can also use `--sudo` to specify this user needs to use sudo.
  * Add additional recipes to the run_list as needed.
  * You're done.