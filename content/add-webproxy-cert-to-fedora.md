+++
title = ""
date = "2013-10-23"
tags = [ "", "" ]
categories = [ "" ]
image = "MIWG_2013_Kirkland_Concours_14.jpg"
+++

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
