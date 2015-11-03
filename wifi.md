"CENS" WiFI
=======

Hardware
---------

The general UCLA wifi networks are enabled at CENS (eduroam, ucla-wifi and ucla-web), but CENS has a legacy wifi network which will provide you with an IP address on the CENS subnet.  Above Steve's cube is a [UniFi AP Pro](https://store.ubnt.com/unifi/unifi-ap-pro.html) (circa early 2013) which provides this service.

Configuration
--------------

While this has been running for a few years with little input if configuration needs to be adjusted, it can be done so by visiting the small VM used for dhcp and the unifi server product (cens.ohmage.org). The unifi web portal is available at [this](https://131.179.144.9:8443/manage) link. If you have questions regarding the setup of this server, consult the relevant [cens-unifi](https://github.com/mobilizingcs-ops/chef-cens-unifi) chef cookbook.