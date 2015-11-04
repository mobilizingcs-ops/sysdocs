Monitoring/Alerting
========

Setup
---------

We use sensu to provide monitoring/alerting. Sensu is completely setup/maintained by chef via [cens-monitor](https://github.com/mobilizingcs-ops/chef-cens-monitor). Adding a new node to be monitored is as simple as dropping some check recipes into the `run_list` of a node in chef.  Any of the check recipes below will resolve the sensu dependencies.

Checks
-----------------

Since that cookbook has many recipes (I followed the model of the project I forked from) I thought it best to lay out the basics of what it does. Any of these recipes can be added to a node's runlist without 

  * [base_checks](https://github.com/mobilizingcs-ops/chef-cens-monitor/blob/master/recipes/base_checks.rb)
    * disk usage and metrics
    * cpu usage/load and metrics
    * memory usage and metrcis
  * [raid_checks](https://github.com/mobilizingcs-ops/chef-cens-monitor/blob/master/recipes/raid_checks.rb)
    * run on physical hosts to alert raid status
  * [mysql](https://github.com/mobilizingcs-ops/chef-cens-monitor/blob/master/recipes/mysql.rb)
    * mysql alive and metrics
  * [rt4_checks](https://github.com/mobilizingcs-ops/chef-cens-monitor/blob/master/recipes/rt4_checks.rb)
    * rt4 alive, runs on support.mobilizingcs.org
  * [ohmage_checks](https://github.com/mobilizingcs-ops/chef-cens-monitor/blob/master/recipes/ohmage_checks.rb)
    * ohmage alive and metrics

If you are inside the CENS network, you can take a look at the current system status using uchiwa (a frontend for sensu) at http://alerts.ohmage.org:3000. user/pass are available from chef-vault.

Alerting
---------

Alerting with sensu is extremely flexible, and we are using only a small part of it.  [This](https://github.com/mobilizingcs-ops/chef-cens-monitor/blob/master/recipes/_mailer_handler.rb) recipe sets up the mailer handlers and emails to send alerts to.  Take a look at the checks to see how occurrences work (eg. you'll be notified every 5 minutes until the service is back up for service X).

Metrics
--------

Check out [analytics](analytics.md) page for details on what/where these metrics are.