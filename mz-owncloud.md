Dropbox
=======

As the mobilize team grew (and mainly, the size of their data needs grew) we opted to move away from the dropbox.com service and self-host an [owncloud](http://owncloud.org) instance.  It's hosted on web.ohmage.org and available from https://dropbox.mobilizingcs.org

Setup
----------

Grab ownloud source and point it at a database. Set up [ldap](ldap.md) so internal mobilize users can log in.

Data Structure
-------

A question better answered by the mobilize group, but there are essentially 3 areas: `Internal` that only internal users have RW access to, `Public` which is where we store publicly available curricula etc. and `Videos`, where we store internal videos of events.  This data is stored on `starbuck`, [synced nightly](backups.md) `cavil` and offloaded to Amazon S3 weekly.