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


Install
----------

To install, run:

    make DESTDIR=/ PREFIX=/usr install


Configuration
-------------

Create a new user, for instance `ovh`

    useradd -m -s /bin/bash ovh
    passwd ovh
    su ovh

Generate a configuration for OVH API requests, run:

    ovh-api-client --initApp

Configure the OVH API application AND the consumer key to use
(see [`ovh-api-client`'s repo](https://github.com/aureooms/ovh-api-client) for more informations).


Add a new crontab (`crontab -e`) to run this script using the right subdomain and domain,
for example (using [`myip`](https://github.com/aureooms/myip)):

    */5 * * * * bash -c 'ovh-dns --target EU --domain example.com --subdomain www --ip "$(myip public)"'

This crontab will check every 5 minutes that the following record targets the right IP address :

    www.example.com.    60    IN A   1.2.3.4

If the target IP address is incorrect, it will update the value.

If you only want to query the API when the machine's IP changes and have `myip`
installed, you can use the more convenient

    */5 * * * * ovh-dns-watch --target EU --domain example.com --subdomain www


Informations
------------

If the A record is not found, it will be created.
