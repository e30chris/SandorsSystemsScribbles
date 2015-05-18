---
layout: post
title: "Add WebProxy Cert to Fedora"
date: 2013-11-29 15:19
comments: false
summary: The Goal - Make Fedora work behind a restrictive idiotic corporate web proxy- Install a corporate cert to make that happen
categories: webstuff
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