---
categories: webstuff
comments: false
date: 2013-11-29T00:00:00Z
summary: The Goal - Make Fedora work behind a restrictive idiotic corporate web proxy-
  Install a corporate cert to make that happen
title: Add WebProxy Cert to Fedora
url: /2013/11/29/add-webproxy-cert-to-fedora/
---

## The Goal

- Make Fedora work behind a restrictive idiotic corporate web proxy
- Install a corporate cert to make that happen



## Get the cert

## Install the cert

~~~
spudBud@pineApplez> openssl x509 -text -in /path/to/proxycert.crt >> /etc/pki/tls/certs/ca-bundle.crt
~~~

## The result

Your corporate overlord has now man in the middled your personal gmail, have fun.