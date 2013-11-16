# SimpleHTTPProxy

A Simple HTTP Proxy like Python's SimpleHTTPServer module.
Works on Python 2.7.

Features:

- easy to use
- easy to customize
- runs fast with minimum footprint
- requires no external modules
- supports HTTP/1.1 persistent connections to some extent
- supports CONNECT tunneling
- supports gzip and deflate compression
- supports IPv6 (for listening you need to edit the script)
- supports sslstrip feature (by SSLStripProxy)
- supports sslbump feature (by SSLBumpProxy, without dynamic certificate generation)


## Usage

```
$ python SimpleHTTPProxy.py 8080
```

or like SimpleHTTPServer:

```
$ python -m SimpleHTTPProxy
```

When the argument is not given, SimpleHTTPProxy uses tcp/8080.

SimpleHTTPProxy provides CONNECT tunneling, in which the data is transfered with no modification.


## Customize

To customize, inherit `SimpleHTTPProxyHandler` class and override the handler methods.

Some examples are included:

- SaveImagesProxy: store all 'image/*' files on the current directory (by keeping their url hierarchy)
- ChangeUAProxy: replace 'User-Agent' header of the client requests
- HideRefererProxy: replace 'Referer' header of the client requests
- RemoveIframeProxy: remove all &lt;iframe&gt; elements from 'text/html' contents
- StripAmazonProxy: force redirect to 'http://www.amazon.co.jp/dp/$ASIN' style url
- CatHeadersProxy: print HTTP headers to stdout

You can use these proxies just as SimpleHTTPProxy:

```
$ python -m SaveImagesProxy
```


## sslstripping by SSLStripProxy

SSLStripProxy inherits SimpleHTTPProxy and implements [sslstrip](http://www.thoughtcrime.org/software/sslstrip/)-like feature.

- work as HTTP proxy (not HTTPS)
- replace https urls to http ones in the responses and remember them
- forward client's HTTP requests to upstream servers as HTTPS requests

Also some examples are included:

- SSLStripSniffProxy: output POST parameters for login pages to stdout (also works via HTTP)
- OffmousedownGoogleProxy: disable onmousedown URL rewriting in Google's Result Pages (HTTPS)

"HTTP Strict Transport Security" policies [RFC 6797] make the browsers always use HTTPS for their domains, so this proxy doesn't work for such cases.


## sslbumpping by SSLBumpProxy

SSLBumpProxy inherits SimpleHTTPProxy and implements [Squid's SslBump](http://wiki.squid-cache.org/Features/SslBump)-like feature.

- work as HTTPS proxy (also as HTTP proxy)
- connect with the client by using SSLBumpProxy's certificate
- note that the client will raise a warning about the certificate

An example is included:

- SSLBurpSniffProxy: output POST parameters for login pages to stdout (also works via HTTP)

Currently, SSLBumpProxy doesn't do dynamic certificate generation.
Just use a prepared certificate.
