# wireshark-how-to
Useful settings and options for Wireshark

## Useful columns
* IP Country > Custom > `ip.geoip.country`
* IP Organization > Custom > `ip.geoip.org`
* Host > Custom > `http.host`
* TLS SNI > Custom > `tls.handshake.extensions_server_name`
* Response for URI > Custom > `http.response_for.uri`

## Filters

* Only show HTTP traffic where a host is included:
`http.host`

* Only show HTTP traffic that is a request:
`http.request`

* Only show HTTP traffic that is a response:
`http.response`

* Only show HTTP traffic with the field Request URI:
`http.response_for.uri`

* Only show TLS traffic with the field Server Name Indication (SNI):
`tls.handshake.extensions_server_name`

* How to show all HTTP responses but exclude OCSP traffic?
  * I wanted to try something like `(ip.proto != 'ocsp') && (http.response_for.uri)` but that didn't work.
  * The solution is `(!ocsp) && (http.response_for.uri)`


## How to get more information in Statistics > Endpoints?
Fields like `Address` or `Packets` might have values in them, but other fields such as `Country`, `City`, `AS Number` and `AS Organization` are empty (Autonomous system number / ASN). How to get this data?

This data can be sourced from MaxMind's GeoLite2 databases. These databases used to be available for free and could be easily downloaded. It is still possible to download them free of charge, however one has to be signed in with a user account on MaxMind's website. The database is available as `.mmdb` and `.csv`. Wireshark can only import the `.mmdb` files.

The last easily downloadable versions have been archived on web.archive.org and are available here (data from 2019-12-24):
* [GeoLite2-City-mmdb.tar.gz](https://web.archive.org/web/20191227182209/https://geolite.maxmind.com/download/geoip/database/GeoLite2-City.tar.gz)
* [GeoLite2-Country-mmdb.tar.gz](https://web.archive.org/web/20191227182412/https://geolite.maxmind.com/download/geoip/database/GeoLite2-Country.tar.gz)
* [GeoLite2-ASN-mmdb.tar.gz](https://web.archive.org/web/20191227182527/https://geolite.maxmind.com/download/geoip/database/GeoLite2-ASN.tar.gz)
* [GeoLite2-City-CSV.zip](https://web.archive.org/web/20191227182816/https://geolite.maxmind.com/download/geoip/database/GeoLite2-City-CSV.zip)
* [GeoLite2-Country-CSV.zip](https://web.archive.org/web/20191227183011/https://geolite.maxmind.com/download/geoip/database/GeoLite2-Country-CSV.zip)
* [GeoLite2-ASN-CSV.zip](https://web.archive.org/web/20191227183143/https://geolite.maxmind.com/download/geoip/database/GeoLite2-ASN-CSV.zip)

Source: [Matomo Forum - Maxmind is changing access to free GeoLite2 databases](https://forum.matomo.org/t/maxmind-is-changing-access-to-free-geolite2-databases/35439/3)


## Log TLS/ SLL keys on Windows systems
* Go to `Environment Variables` and add a new system variable
* Name: `SSLKEYLOGFILE`
* Location, e.g.: `C:\logs\ssl-log.txt`

More information here: https://www.comparitech.com/net-admin/decrypt-ssl-with-wireshark/
