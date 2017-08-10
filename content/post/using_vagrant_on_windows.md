---
title: "Using Vagrant on Windows"
date: "2013-12-17"
tags: [ "vagrant", "winders" ]
categories: [ "workstation" ]
image: "MIWG_2013_Kirkland_Concours_14.jpg"
---

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

{{< highlight bash >}}
&nbsp;
C:\Windows\system32\>vagrant box add centosPupLabs http://puppet-vagrant-boxes.puppetlabs.com/centos-64-x64-vbox4210-nocm.box
&nbsp;
{{< /highlight >}}

What that does:

{{< highlight bash >}}
&nbsp;
C:\Windows\system32\>vagrant box add boxName http://url.box
&nbsp;
{{< /highlight >}}

- Downloads the .box file to the Vagrant boxes directory
- nocm = No "configuration management" installed
- We will use a Vagrant plugin called Oscar to install and configure the PuppetMaster and Agent in a separate post.


## Start the Vagrant Box

{{< highlight bash >}}
&nbsp;
C:\stuff\vagrants\>vagrant init centosPupLabs
&nbsp;
{{< /highlight >}}

{{< highlight bash >}}
&nbsp;
C:\stuff\vagrants\>vagrant up
&nbsp;
{{< /highlight >}}

{{< highlight bash >}}
&nbsp;
C:\stuff\vagrants\>vagrant ssh
&nbsp;
{{< /highlight >}}

What that does:

- Tells Vagrant to write a VagrantConfig file in `C:\stuff\vagrants\`
- Vagrant then boots the centosPupLabs VirtualBox machine
- SSH into the VM
  - Must have Cygwin or Git installed
  - You can also use PuTTY
