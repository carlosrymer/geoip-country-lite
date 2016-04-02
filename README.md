GeoIP-country-lite
==================

A fork of GeoIP-lite, geoip-country-light excludes all city data, which is the bulk of the data that GeoIP-lite stores into RAM. As a result, geoip-country-light is less RAM-intensive. It also doesn't require you to update any data.

Software is written by Philip Tellis philip@bluesmoon.info, latest version is available at https://github.com/bluesmoon/node-geoip

This module is a native NodeJS API for the **country** GeoLite data from MaxMind.

This product includes GeoLite data created by MaxMind, available from http://maxmind.com/

Software is written by Philip Tellis <philip@bluesmoon.info>, latest version is available at https://github.com/bluesmoon/node-geoip

Introduction
------------

MaxMind provides a set of data files for IP to Geo mapping along with opensource libraries to parse and lookup these data files.
One would typically write a wrapper around their C API to get access to this data in other languages (like JavaScript).

geoip-country-light instead attempts to be a fully native JavaScript library.  A converter script converts the CSV files from MaxMind into
an internal binary format (note that this is different from the binary data format provided by MaxMind).  The geoip module uses this
binary file to lookup IP addresses and return the country it maps to.

Both IPv4 and IPv6 addresses are supported, however since the GeoLite IPv6 database does not currently contain any city or region
information, city and region lookups are only supported for IPv4.

Philosophy
----------

I was really aiming for a fast JavaScript native implementation for geomapping of IPs.  My prime motivator was the fact that it was
really hard to get libgeoip built for Mac OSX without using the library from MacPorts.

Why geoip-country-light
----------------------

So why are we called geoip-country-light?  `npm` already has a [geoip package](http://search.npmjs.org/#/geoip) which provides a JavaScript binding around libgeoip from MaxMind.  The `geoip` package is fully featured and supports everything that the MaxMind APIs support, however, it requires `libgeoip` to be installed on your system.

`geoip-country-light` on the other hand is a fully JavaScript implementation.  It is not as fully featured as `geoip` however, by reducing its scope, it is about 40% faster at doing lookups.  On average, an IP to Location lookup should take 20 microseconds on a Macbook Pro.

IPv4 addresses take about 6 microseconds, while IPv6 addresses take about 30 microseconds.

Synopsis
--------

```javascript
var geoip = require('geoip-country-light');

var ip = "207.97.227.239";
var geo = geoip.lookup(ip);

console.log(geo);
{ 
  country: 'US'
}
```

Installation
------------
### 1. get the library

    $ npm install geoip-country-light

### 2. update data

    $ npm run-script updatedb

API
---

geoip-country-light is completely synchronous. There are no callbacks involved. All blocking file IO is done at startup time, so all runtime calls are executed in-memory and are fast. Startup may take up to 200ms while it reads into memory and indexes data files.

### Looking up an IP address ###

If you have an IP address in dotted quad notation, IPv6 colon notation, or a 32 bit unsigned integer (treated
as an IPv4 address), pass it to the `lookup` method. Note that you should remove any `[` and `]` around an
IPv6 address before passing it to this method.

```javascript
var geo = geoip.lookup(ip);
```

If the IP address was found, the `lookup` method returns an object with the following structure:

```javascript
{
   country: 'XX' // 2 letter ISO-3166-1 country code
}
```

The actual values for the `range` array depend on whether the IP is IPv4 or IPv6 and should be
considered internal to `geoip-country-light`.  To get a human readable format, pass them to `geoip.pretty()`

If the IP address was not found, the `lookup` returns `null`

Caveats
-------

This package includes the GeoLite database from MaxMind.  This database is not the most accurate database available,
however it is the best available for free.  You can use the commercial GeoIP database from MaxMind with better
accuracy by buying a license from MaxMind, and then using the conversion utility to convert it to a format that
geoip-country-light understands.  You will need to use the `.csv` files from MaxMind for conversion.


References
----------
  - <a href="http://www.maxmind.com/app/iso3166">Documentation from MaxMind</a>
  - <a href="http://en.wikipedia.org/wiki/ISO_3166">ISO 3166 (1 & 2) codes</a>

Copyright
---------

`geoip-country-light` is Copyright 2011-2012 Philip Tellis <philip@bluesmoon.info> and the latest version of the code is
available at https://github.com/bluesmoon/node-geoip

License
-------

There are two licenses for the code and data.  See the [LICENSE](https://github.com/carlos/node-geoip-country/blob/master/LICENSE) file for details.
