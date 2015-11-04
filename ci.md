Continuous Integration, Deployment and Jenkins
=======

Since we do a load of ad-hoc developing internally, we have a similarly ad-hoc integration/deployment environment that mostly makes use of Jenkins.

Setup
------

The nginx/ssl bits of the jenkins setup are handled via [chef](https://github.com/mobilizingcs-ops/chef-cens-jenkins). The rest is job-level in jenkins!

Hosts
------

The cens jenkins environments uses two hosts, a linux VM and a mac mini build server (provided by ECTG/OIT @ UCLA). The jenkins server is available at https://ci.ohmage.org and the mac slave can be found at ci-mac.ohmage.org while inside the CENS network.  

Jobs
-----

All of the mobilize/ohmage frontend builds are publicly viewable at https://ci.ohmage.org/. Few other builds exist.

Deploying
----------

A list of the current jobs, and how to go about deploying them:

  * `admin, author, campaigns, classes, dashboard, demo, documents, monitor, navbar, responses, teacher`: deployed to test.mobilizingcs.org on GH push, deployable to other servers by logging in to ci.ohmage.org and building with the parameter matching the hostname you'd like to deploy to (eg. `lausd`)
  * `mobilizR`: continuously integrated into the rstudio.mobilizingcs.org server by deploying to ocpu.ohmage.org. Good Luck with that!
  * `plotapp`: continuously integrated into dev.opencpu.org by Jeroen's toolset, deployed to production/ocpu.ohmage.org on build (job is not public).
  * `ohmageX`: artifacts are dropped off to https://apps.ohmage.org

