---
categories: vagrant
comments: false
date: 2013-11-12T00:00:00Z
summary: The Goal - Setup Vagrant - Start a Vanilla Ubuntu box
title: vanilla vagrant setup
url: /2013/11/12/vanilla-vagrant-setup/
---

## The Goal
- Setup Vagrant
- Start a Vanilla Ubuntu box




## The Links

- Vagrant - [link](http://www.vagrantup.com/)

## Vagrant Setup

### Grab the latest [here](http://downloads.vagrantup.com/)

### Install it

### Thats it

## Adding Boxes

When you add a box to your local puter it downloads the VM image to your home dir like so:

~~~
vagrant box add name url
~~~
~~~
spudBud@pineApplez> vagrant box add precise32 http://files.vagrantup.com/precise32.box
~~~
This downloads the VM image into the /boxes dir:

~~~
spudBud@pineApplez> ~/Codestuff/vagrants/PuppetMaster $la ~/.vagrant.d/boxes/
total 
drwxr-xr-x  7 spudBud  staff   238B Aug 27 17:38 ./
drwxr-xr-x  7 spudBud  staff   340B Aug 14 16:46 ../
drwxr-xr-x  3 spudBud  staff   102B Aug 27 17:38 centos6min/
drwxr-xr-x  3 spudBud  staff   102B Aug 27 15:51 lucid32/
drwxr-xr-x  3 spudBud  staff   102B Aug 27 16:12 precise32/
drwxr-xr-x  3 spudBud  staff   102B Jun 13 16:33 raring/
drwxr-xr-x  3 spudBud  staff   102B Jul  8 11:15 ubuntu/
~~~

With each box dir looking like so:

~~~
spudBud@pineApplez> ~/Codestuff/vagrants/PuppetMaster $la ~/.vagrant.d/boxes/centos6min/virtualbox/
total 742368
drwxr-xr-x  2 spudBud  staff   238B Aug 27 17:38 ./
drwxr-xr-x  3 spudBud  staff   102B Aug 27 17:38 ../
-rw-r--r--  1 spudBud  staff   505B Aug 27 17:37 Vagrantfile
-rw-------  1 spudBud  staff   362M Aug 27 17:38 box-disk1.vmdk
-rw-------  1 spudBud  staff   121B Aug 27 17:37 box.mf
-rw-------  1 spudBud  staff    13K Aug 27 17:38 box.ovf
-rw-r--r--  1 spudBud  staff    25B Aug 27 17:38 metadata.json
~~~
Once you download a box the base image stays in this directory even if you destroy the VM you create.  The only way to completely remove a box from your puter (and remove it totally from the ~/.vagrant.d/boxes dir) is by running 

~~~
spudBud@pineApplez> vagrant box remove precise64 virtualbox
~~~

The two arguments are the logical name of the box and the provider of the box.


## Vanilla Vagrant
Start a vanilla Ubuntu like so:

### Download the Box

~~~
spudBud@pineApplez> ~/Codestuff/vagrants $vagrant box add precise32 http://files.vagrantup.com/precise32.box
~~~

### Create the dir

~~~
spudBud@pineApplez> ~/Codestuff/vagrants $mkdir precise32 && cd precise32
~~~

### Create the Vagrantfile

~~~
spudBud@pineApplez> ~/Codestuff/vagrants/precise32 $vim Vagrantfile
~~~
~~~
Vagrant.configure("2") do |config|
  config.vm.box = "precise32"
end
~~~

This creates a basic Vagrantfile to start up a machine.  If you use 'vagrant init' you get a really long file with a bunch of crap you don't need to read that does what this does.  KISS!

### Start the VM

~~~
spudBud@pineApplez> ~/Codestuff/vagrants/precise32 $vagrant up
~~~

### Login

~~~
spudBud@pineApplez> ~/Codestuff/vagrants/precise32 $vagrant ssh
~~~

You are now logged into a Vanilla Vagrant box. 
