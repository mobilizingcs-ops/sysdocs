Analytics
=======

Tied into the monitoring via sensu, we throw a bunch of metrics to a graphite server for viewing with grafana.

Setup
----------

The graphite/grafana install are co-mingled on the alerts server with sensu server. See `master, master-graphite and master-grafana` recipes in [cens-monitor](https://github.com/mobilizingcs-ops/chef-cens-monitor/tree/master/recipes) composition.

Viewing
--------

If you are on the CENS network, visit http://alerts.ohmage.org to start using grafana.  You can load the `mobilize-status` dashboard for a good idea of what we are collecting and to extend for your own purposes.  Don't save over the mobilize-status dashboard.

Extending
----------

The metrics being gathered for this system are rather rudimentary, and can be extended with little effort. Here's an example:

  * [ohmage-metrics](https://github.com/mobilizingcs-ops/chef-cens-monitor/blob/master/recipes/ohmage_checks.rb#L31-L37)
    * [a script](https://github.com/mobilizingcs-ops/chef-cens-monitor/blob/master/files/default/plugins/ohmage-metrics.rb), run by sensu every 60 seconds
    * script output in graphite format (`highly.nested.item.name count`)
    * sensu uses the [graphite handler](https://github.com/mobilizingcs-ops/chef-cens-monitor/blob/master/recipes/_graphite_handler.rb) to pipe that output to the graphite server.
    * data gets timestamp and is viewable later on!

Monitoring
-----------

If you haven't seen the page on [sensu](sensu.md), take a look at that now.