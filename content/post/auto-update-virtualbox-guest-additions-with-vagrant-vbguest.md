---
title = "Auto Update VirtualBox Guest Additions with vagrant-vbguest"
date = "2013-11-12"
tags = [ "vagrant", "workstation" ]
categories = [ "workstation" ]
image = ""
---

## The Goal

- Keep the VirtualBox guest additions at the latest version using vagrant-vbguest built by dotless-de.



## The Links

- Vagrant - [link](http://www.vagrantup.com/)

- Vagrant vbguest - [link](https://github.com/dotless-de/vagrant-vbguest)

- dotless-de - [link](https://github.com/dotless-de)


## vagrant-vbguest

A beautifully simple Vagrant plugin to manage the guest additions on VirtualBox.

## install

~~~
spudBud@pineApplez> ~/Codestuff/vagrants/PuppetMaster $vagrant plugin install vagrant-vbguest
~~~

## bootup usage

vagrant-vbguest will run on every `vagrant up` or on a `vagrant reload` unless you specifically tell it not to.

## running VM usage

~~~
spudBud@pineApplez> vagrant vbguest --status
~~~
