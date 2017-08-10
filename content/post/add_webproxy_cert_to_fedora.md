---
title: "Add WebProxy Cert to Fedora"
date: "2013-11-29"
tags: [ "fedora", "linux", "bullshit", "workstation", "techdebt", "oldcorp" ]
categories: [ "linux" ]
image: ""
---

## The Goal

- Make Fedora work behind a restrictive idiotic corporate web proxy
- Install a corporate cert to make that happen



## Get the cert

## Install the cert

{{< highlight bash >}}
&nbsp;
spudBud@pineApplez> openssl x509 -text -in /path/to/proxycert.crt >> /etc/pki/tls/certs/ca-bundle.crt
&nbsp;
{{< /highlight >}}

## The result

Your corporate overlord has now man in the middled your personal gmail, have fun.
