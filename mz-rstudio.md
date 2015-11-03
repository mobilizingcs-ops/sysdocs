Mobilize RStudio
=======

Mobilize uses a self-hosted [rstudio](https://www.rstudio.com) install. It is available at https://studio.mobilizingcs.org/ (accounts are synced with the mobilize ohmage server, lausd.mobilizingcs.org).

Setup
----------

This server is maintained entirely by chef resources, see [this](https://github.com/mobilizingcs-ops/chef-cens-rstudio/).


Packages
---------

This server is mainly available to provide the required R resources for the Mobilize IDS curriculum.  This curriculum uses a bespoke R package called [MobilizR](https://github.com/mobilizigncs/mobilizr). This package is integrated using our [ci strategies](ci.md). To make sense of the [r package dependencies](https://github.com/mobilizingcs-ops/chef-cens-rstudio/blob/master/recipes/default.rb#L40) and [autoload](https://github.com/mobilizingcs-ops/chef-cens-rstudio/blob/master/recipes/default.rb#L36) this package for users, the package list is managed in the chef resources, as linked.

Account Sync
-------------

A weird one. The account usernames and password hashes are synced from lausd.mobilizingcs.org every 15 minutes using a team of scripts on lausd.mobilizingcs.org and this password (namely, the actions on this server make use of the `chpasswd` linux util). More info on this can be found on the [ohmage mobilize-specific](mz-ohmage.md) page.

Student/Teacher history files
------------------------------

RStudio very nicely stores the command history of users.  Since this is a research project, we store these and cross-reference as we'd like to make use of this data for analysis later on. This is handled with [this script](https://github.com/stevenolen/ohmagetools/blob/master/ohmage_rstudio_links/per_class_history_database.rb).