Mobilize Issue Tracker
=======

Mobilize uses a self-hosted [rt4](https://www.bestpractical.com/rt/) install. It is available at https://support.mobilizingcs.org/ (log in with your [ldap](ldap.md) account).

Setup
----------

A bit of a bespoke setup. This was set up pre-chef and is a bit quirky so I opted not to re-invent the wheel! The rt4 service and nginx reverse proxy are handled with [chef](https://github.com/mobilizingcs-ops/chef-cens-rt4) but the remainder was installed via `apt-get`.

  * `postfix` for outgoing mail: postfix is configured to use support@mobilizingcs.org with the google app smtp server.
  * `fetchmail` for incoming mail: mail sent to support@, recruit@ or eval@ is monitored by `fetchmail` (check /root/bin/fetchmail_multi.sh) and placed into rt4.

Text/Voicemail Forwarding
-------------------------

Because at some point someone (probably me) thought this was a good idea, we accept support requests via text messages and voicemail.  These are both configured by logging in to google.com/voice with support@mobilizingcs.org credentials. In the settings tab you'll find settings for "do not disturb" being permanently on, voicemail to send to email and text messages to forward to email (which are all then picked up by fetchmail). This is craziness.

Queues
-------

At this time, we have 12 queues (tech, management, ids_curriculum to name a few) that messages can be sorted into.  Save for eval@ and recruit@, all messages originate in the General queue where the general queue admins can re-assign them. The General queue is the only queue that has users managed directly (`Tools -> Configuration -> Queues -> Select, select General, click Watchers tab and view AdminCc users`). All other queues have AdminCc watchers set up based on groups, which you can manage at `Tools -> Configuration -> Groups -> Select`.

Scrips
-------

rt4 uses "scrips" (yes, spelled like that) to customize behavior. Most scrips in use are default behavior, with a few exceptions.  `Tools -> Configuration -> Global -> Scrips` will lead you to the list of scrips.  The non-default scrips are 12,13,14 and are used to provide additional notifications to queue members when queues,owners and statuses change.