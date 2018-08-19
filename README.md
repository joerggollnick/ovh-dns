OVH DNS
=======

Use this script in a cron to update a given A record in your DNS zone, using OVH API.


Dependencies
----------

Depends on the following executables
(might need manual installation):

  - [`ovh-api-client`](https://github.com/aureooms/ovh-api-client)
  - [`jq`](https://stedolan.github.io/jq)
  - [`bash`](https://www.gnu.org/software/bash)

Depends on the following core utilities
(likely to be installed by default on most GNU/Linux distributions):

  - [`cut`](http://man7.org/linux/man-pages/man1/cut.1.html)
  - [`ping`](http://man7.org/linux/man-pages/man8/ping.8.html)


Initialize
----------

### Install

To install, run:

    make DESTDIR=/ PREFIX=/usr install


### First initialization

First in order to initialize the informations for OVH API requests, run:

    ovh-api-client --initApp

You will be prompted to configure the OVH API application and the consumer key to use
(see [`ovh-api-client`'s repo](https://github.com/aureooms/ovh-api-client) for more informations).


Configuration
-------------

Just add a new crontab to run this script using the right subdomain and domain,
for example (using [`myip`](https://github.com/aureooms/myip)):

    * * * * * ovh-dns --domain mydomain.com --subdomain home --ip "$(myip public)"

This crontab will check every minute that the following record targets the right IP address :

    home.mydomain.com.    60    IN A   1.2.3.4

If the target IP address is incorrect, it will update the value.


Informations
------------

If the A record is not found, it will be created.
