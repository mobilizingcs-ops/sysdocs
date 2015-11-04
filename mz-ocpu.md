OpenCPU
======

A deploy of [opencpu](http://opencpu.org) is available for the mobilize deployment ohmage server at http://ocpu.ohmage.org.  

Setup
======

Follow the install instructions [here](https://www.opencpu.org/download.html) and then install the [plotapp](https://github.com/mobilizingcs/plotapp) package from github. Take a look at the [cens-ohmage](https://github.com/mobilizingcs-ops/chef-cens-ohmage/blob/master/templates/host-lausd.mobilizingcs.org/ohmage-nginx.conf.erb#L65-L67) chef resources for some hints on reverse-proxying from the ohmage server to make the plotapp easy available for your ohmage users.