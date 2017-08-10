---
title: "Using Ansible to deploy a PuppetMaster"
date: "2014-06-13"
tags: [ "ansible", "puppet" ]
categories: [ "ansible" ]
image: ""
---

## The Problem
Need to deploy a new Puppet Enterprise cluster with a PuppetMaster, PuppetConsole and a PuppetDB while avoiding typos and misconfigurations.  Also need to deploy to several environments using a consistent configuration.

## The Solution
Use Ansible to deploy Puppet onto freshly built servers that contain just a SysAdmins SSH public key.

## The Goal
A repeatable and documented way to deploy the very finicky Puppet installer in any environment needed from local vagrants to the clouds of vSphere, AWS, RackSpace or Digital Ocean.

#### Note: this playbook is still a work in progress.  Most of the issues are fighting Puppets insane complexity of getting installed correctly with a few small Ansible bugs sprinkled in.


---

## GitHub Repo
[github.com/e30chris/Ansible-PuppetEnterpriseDeploy](https://github.com/e30chris/Ansible-PuppetEnterpriseDeploy)


---
## Playbook notes

## Setting the variables for each environment

{{< highlight bash >}}
&nbsp;
sandor@pineapplez:$ cd ~/Ansible-PuppetEnterpriseDeploy/
sandor@pineapplez:$ cat group_vars/all
---
# common variables for PuppetMaster Deployment
# format for this file
# variable: fact
#
# version of Puppet Enterprise
pe_version: 3.2.3

# os family
os_family: debian
os_version: 7
os_arch: amd64

# pe installer
pe_installer: puppet-enterprise-{{ pe_version }}-{{ os_family }}-{{ os_version }}-{{ os_arch }}

# hostnames
pupmaster_hostname: pupmaster.argo.local
pupdb_hostname: pupdb.argo.local
pupconsole_hostname: pupconsole.argo.local

# IP's
pupmaster_ip: 10.0.0.10
pupdb_ip: 10.0.0.11
pupconsole_ip: 10.0.0.12

# dns salt hostnames
pupmaster_salt_hostnames: puppet,puppetmaster
pupdb_salt_hostnames: puppetdb
pupconsole_salt_hostname: pupconsole

# passwords
console_auth_db_pass: arandompasswordthatneedschangedhere
console_db_pass: arandompasswordthatneedschangedhere
pupdb_db_pass: arandompasswordthatneedschangedhere
db_root_pass: arandompasswordthatneedschangedhere

&nbsp;
{{< /highlight >}}

To keep it simple only the values that should be changed are assigned variables.  Everything else is left with the Puppet defaults in the answer files.

---

## Tasks to be run on all servers

Puppet needs very perfect name resolution between all agents and the PuppetMaster.  Having a **perfect** hosts file on each server is required.

{{< highlight bash >}}
&nbsp;
- name: Ensure common etc hosts file
  template:
    src="../templates/hosts.j2"
    dest=/etc/hosts
&nbsp;
{{< /highlight >}}

To keep things simple all the installation tasks will run out of a /puppetinstall directory.  This should be in the user home dir that is running the installer.  The examples here use root but the SysAdmin Bible states never use root always use **sudo**.

{{< highlight bash >}}
&nbsp;
- name: Ensure Puppet installer directory present
  file:
    path=/root/puppetinstall
    state=directory

- name: Ensure install log present
  file:
    path=/root/puppetinstall/pupconsole_install.log
    owner=root
    state=touch
&nbsp;
{{< /highlight >}}


When you download the PE tarball you can grab the download url from S3.  This grabs the version you need and extracts it into the installer dir.

{{< highlight bash >}}
&nbsp;
- name: Ensure PE tarball present
  get_url:
    url=https://s3.amazonaws.com/pe-builds/released/{{ pe_version }}/{{ pe_installer }}.tar.gz
    dest=/root/peinstaller.tar.gz
    force=no

- name: Ensure Puppet untar'd to install directory
  unarchive:
    copy=no
    src=/root/peinstaller.tar.gz
    dest=/root/puppetinstall
&nbsp;
{{< /highlight >}}
---

## Tasks that run on each server role

Each server then runs the same basic installer command with a few things named for each role like the installer log.

Here is the PuppetMasters install-

{{< highlight bash >}}
&nbsp;
---
# Tasks for PupMaster
- name: Ensure install log present
  file:
    path=/root/puppetinstall/pupmaster_install.log
    owner=root
    state=touch

- name: answer file for PupMaster
  template:
    src="../templates/pupmaster.answer.j2"
    dest=/root/puppetinstall/pupmaster.answer
    owner=root

- name: run the pe installer
  command: /root/puppetinstall/{{ pe_installer }}/puppet-enterprise-installer -a /root/puppetinstall/pupmaster.answer -l /root/puppetinstall/pupmaster_install.log
&nbsp;
{{< /highlight >}}
