Mobilize ohmage
=======

Mobilize uses (and futher extends/develops) [ohmage](http://ohmage.org) as a data collection/analysis tool to facilitate the Mobilize curriculum. 

Setup
----------

Many of what makes up the Mobilize ohmage server (https://lausd.mobilizingcs.org) is handled via [chef resources](https://github.com/mobilizingcs-ops/chef-cens-ohmage). Currently, the frontends and new versions of the ohmage server are actually deployed by hand, since we're a bit particular about the stability of this box. We use nginx as a reverse proxy to handle ssl termination and mysql as the database server. 

Account Sync
-------------

A weird one. The account usernames and password hashes sync to rstudio.mobilizingcs.org every 15 minutes. The script that runs is available at `/root/bin/sync_accounts_send.sh`.  It dumps the `user` table from the ohmage database and diffs it from the user/password hash combos we already have and sends the differences to the rstudio server.

Account Privileges
-------------------

The current setup does not sync class lists/accounts for LAUSD teachers and students, so it is impertative that we have resources to help teachers get started quickly. This is done, awkwardly, via a number of methods. There are distinct hopes to change this process in 2016.

  * The [Teacher Tool](https://github.com/mobilizingcs/teacher) handles creation of classes and users via CSV uploads (exported from MISIS). It also handles having teachers accept a policy for these privileges and request the permission.
  * The permission request is done by teachers submitting a survey to a particular campaign in the system. Only authorized teachers can submit this survey
  * `/root/bin/addPrivs.sh` searches the ohmage survey responses for requests and grants the privilege to teachers who have submitted.

Deployment/Teacher Updates
---------------------------

To keep our internal Mobilize team in the loop on data collection (as well as to hopefully spark some competition between teachers!) we send out periodic emails with this content.

  * The internal deployment updates are handled via [this script](https://github.com/stevenolen/ohmagetools/blob/master/deployment_update/deploy_to_wiki.rb) (note the use of the `dsummary` mysql store procedure available in the directory on github next to this script). The resulting table is pushed to the mobilize wiki and the team is notified once a week.
  * The teachers are updated based on the Top 15 of their class type, handled via [this script](https://github.com/stevenolen/ohmagetools/blob/master/deployment_update/teacher_update.rb).
  * simple mailing groups are used for both of these (eg. teacher_list@mobilizingcs.org) and the lists maintained in [Google Apps](https://google.com/a/ohmage.org)
