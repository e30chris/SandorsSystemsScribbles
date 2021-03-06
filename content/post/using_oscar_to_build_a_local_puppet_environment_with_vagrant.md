---
title: "Using Oscar to Build a Local Puppet Environment with Vagrant"
date: "2013-12-20"
tags: [ "puppet", "vagrant" ]
categories: [ "workstation" ]
image: ""
---

## The Goal

- Install Oscar
- Use Oscar to build a local PuppetMaster & Agent config
- Use Vagrant to start the PuppetMaster & Agent



## Install

- VirtualBox - [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
- Vagrant - Oscar works with Vagrant version 1.3.4 - [http://downloads.vagrantup.com/tags/v1.3.4](http://downloads.vagrantup.com/tags/v1.3.4)
- Oscar - [https://github.com/adrienthebo/oscar](https://github.com/adrienthebo/oscar)

{{< highlight bash >}}
&nbsp;
sandor@pineApplez> vagrant plugin install oscar
&nbsp;
{{< /highlight >}}


## Add a Vagrant Box

{{< highlight bash >}}
&nbsp;
sandor@pineApplez> vagrant box add centosPupLabs http://puppet-vagrant-boxes.puppetlabs.com/centos-64-x64-vbox4210-nocm.box
&nbsp;
{{< /highlight >}}

What that does:

{{< highlight bash >}}
&nbsp;
sandor@pineApplez> vagrant box add boxName http://url.box
&nbsp;
{{< /highlight >}}

- Downloads the .box file to the Vagrant boxes directory
- nocm = No "configuration management" installed


## Create a Vagrant environment with Oscar

Give Vagrant a Puppet Enterprise installer location & version.

{{< highlight bash >}}
&nbsp;
sandor@pineApplez> vim config/pe_build.yaml

---
 pe_build:
   version: "3.1.3"
   #download_root: 'http://s3.amazonpebucket.com'
&nbsp;
{{< /highlight >}}

Set the download_root or manually add the installer with

{{< highlight bash >}}
&nbsp;
sandor@pineApplez> vagrant pe-build copy puppet-enterprise-3.1.3-el-6-x86_64.tar.gz
&nbsp;
{{< /highlight >}}

{{< highlight bash >}}
&nbsp;
sandor@pineApplez> vagrant oscar init
&nbsp;
{{< /highlight >}}

{{< highlight bash >}}
&nbsp;
sandor@pineApplez> vagrant oscar init-vms \
                   --master master=centosPupLabs \
                   --agent firstagent=centosPupLabs
                   --agent secondagent=centosPupLabs
&nbsp;
{{< /highlight >}}

{{< highlight bash >}}
&nbsp;
sandor@pineApplez> vagrant up
&nbsp;
{{< /highlight >}}

What that does:

- Creates a Vagrantfile that is customized by Oscar
- Adds a PuppetMaster and 2 agents to the Vagrant environment
- Uses the PuppetLabs Centos box as the OS for all 3 VMsgs
- Starts the group of VMs


## Login to the VMs

{{< highlight bash >}}
&nbsp;
sandor@pineApplez> vagrant ssh master
sandor@pineApplez> vagrant ssh firstagent
sandor@pineApplez> vagrant ssh secondagent
&nbsp;
{{< /highlight >}}



## The result

- 3 running VMs with a PuppetMaster and 2 agents
- Both Agents are authenticated with the Master
- You can now run your Puppet code
