+++
title = ""
date = "2013-10-23"
tags = [ "", "" ]
categories = [ "" ]
image = "MIWG_2013_Kirkland_Concours_14.jpg"
+++

## The Goal

- Install and configure VirtualBox & Vagrant on your Windows dev box
- Download and start a Linux & Windows Vagrant Box.



## Install

- VirtualBox - [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
- Vagrant - [http://www.vagrantup.com/downloads](http://www.vagrantup.com/downloads)
  - Download the .msi
  - Right click and `Install as Administrator`


## Add a Vagrant Box

- Open a Command Prompt and `Run as Administrator`

~~~
C:\Windows\system32\>vagrant box add centosPupLabs http://puppet-vagrant-boxes.puppetlabs.com/centos-64-x64-vbox4210-nocm.box
~~~

What that does:

~~~
C:\Windows\system32\>vagrant box add boxName http://url.box
~~~

- Downloads the .box file to the Vagrant boxes directory
- nocm = No "configuration management" installed
- We will use a Vagrant plugin called Oscar to install and configure the PuppetMaster and Agent in a separate post.


## Start the Vagrant Box

~~~
C:\stuff\vagrants\>vagrant init centosPupLabs
~~~

~~~
C:\stuff\vagrants\>vagrant up
~~~

~~~
C:\stuff\vagrants\>vagrant ssh
~~~

What that does:

- Tells Vagrant to write a VagrantConfig file in `C:\stuff\vagrants\`
- Vagrant then boots the centosPupLabs VirtualBox machine
- SSH into the VM
  - Must have Cygwin or Git installed
  - You can also use PuTTY
