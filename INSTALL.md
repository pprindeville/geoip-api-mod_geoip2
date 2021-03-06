# Installation

## Requirements

Apache 2.0.40 or higher installed  If using RPM, the httpd-devel package
containing apxs may need to be installed [GeoIP 1.4.3 or higher
installed](https://github.com/maxmind/geoip-api-c/releases).

## Building

To build mod_geoip as a dynamically loadable module:

```
apxs -i -a -L/usr/local/lib -I/usr/local/include -lGeoIP -c mod_geoip.c
```

`-I/usr/local/include` is where the GeoIP.h header file is installed.
`-L/usr/local/lib` is where the libGeoIP library is located.

This will put the correct LoadModule statement.

## Configuration

To enable the module, place

```
<IfModule mod_geoip.c>
  GeoIPEnable On
</IfModule>
```

inside your httpd.conf file. The `<IfModule>` block is not required but is
recommended.

You can optionally specify the data file by using:

```
<IfModule mod_geoip.c>
  GeoIPEnable On
  GeoIPDBFile /usr/tmp/GeoIP.dat
</IfModule>
```


See the README.md file for more details on the configuration

## Troubleshooting

On RedHat 9, apxs places

```
LoadModule geoip_module       /usr/lib/httpd/modules/mod_geoip.so
```

Inside a

```
<IfModule worker.c>
</IfModule>
```
Block.

If mod_geoip2 doesn't work, try taking the LoadModule geoip_module line outside
the `<IfModule worker.c>` block.

If you get the following errors:

```
parse error before "geoip_module"
warning: data definition has no type or storage class
```

Or

```
syntax error before "geoip_module"
```

Make sure that the apxs script belongs to the Apache server you are using.
These error messages result from using a Apache 1.3 apxs script with a Apache
2.x server.

If you get a `libGeoIP.so.1: cannot open shared object file: No such file or
directory` error, add `/usr/local/lib` to `/etc/ld.so.conf` then run
`/sbin/ldconfig /etc/ld.so.conf`
