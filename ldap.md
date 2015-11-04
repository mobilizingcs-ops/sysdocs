LDAP
=======

We make use of openldap as a nice and simple ldap server at ldap.ohmage.org

Setup
----------

Currently all bespoke. Install openldap from the apt repos.

Using this LDAP
---------------

This LDAP server is accessible only via accepted hosts (named in `/etc/hosts.allow`) and only from inside the CENS network.  Once you've added a new host you can configure it to access `ldap://ldap.ohmage.org:389`. No auth is required, but an admin user is available.

Known Hosts
------------

At the writing, a number of hosts use the ldap setup: `dropbox.mobilizingcs.org`, `wiki.mobilizingcs.org`, `support.mobilizingcs.org` and a number of internal hosts use ldap for linux auth. 

Maintenance
-----------

I've found [Apache Directory Studio](https://directory.apache.org/studio/) to be the nicest app for account additions/modifications.  Admin creds for the ldap server are needed, and this can only be used from inside the CENS network.

Password Resets
---------------

A nice little php script is available at https://web.ohmage.org/selfserv/ to use for self-service password resets.