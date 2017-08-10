---
title: "Using Ansible to Align Passwords on Servers"
date: "2014-07-17"
tags: [ "ansible", "security", "bullshit", "techdebt", "oldcorp" ]
categories: [ "ansible" ]
image: ""
---

## The Problem:
A group of servers get their passwords updated every 3 months with a new shared password.  The servers passwords are then inconsistently updated leaving a group of servers with one of several possible user passwords.  

## The Goal:
Use Ansible to attempt to login to each server using each of the possible passwords to find the correct one and log it.  The final step of the playbook will have Ansible logging into each server using the found to be correct password and updating each server to a new shared password.

_**note - using the same root password on all your servers is a terrible idea and completely against the SysAdmin Bible.  Use random passwords and keys!**_



## HowTo:

  * Download the playbook from
    * GitHub - [repo](https://github.com/e30chris/Ansible-AlignPassword)
    * Galaxy - [repo](https://galaxy.ansible.com/list#/roles/1134)
  * Create a branch to edit the variables for your environment

  {{< highlight bash >}}
  &nbsp;
  sandor@pineapplez:$ cd ~/Codestuff/ansibles/Ansible-AlignPassword
  sandor@pineapplez:$ git checkout -b qatenv    
  &nbsp;
  {{< /highlight >}}

  * ansible-vault the file that will contain the passwords.

  {{< highlight bash >}}
  &nbsp;
  sandor@pineapplez:$ ansible-vault encrypt group_vars/all
  &nbsp;
  {{< /highlight >}}

  * Edit the vars for the environment

  {{< highlight bash >}}
  &nbsp;
  sandor@pineapplez:$ ansible-vault edit group_vars/all
  &nbsp;
  {{< /highlight >}}

  * Add the inventory to hosts
  * Verify Ansible connect with ping pong

  {{< highlight bash >}}
  &nbsp;
  sandor@pineapplez:$ ansible -i hosts all -m ping
  &nbsp;
  {{< /highlight >}}

  * Run the playbook

  {{< highlight bash >}}
  &nbsp;
  sandor@pineapplez:$ ansible-playbook -i hosts site.yml -vv
  &nbsp;
  {{< /highlight >}}

## Common Task Actions

  * Attempt to login to each server using all possible passwords
  * Register which passwords work on each server
  * Set a new shared password for every server  


## Variables

Go here:

{{< highlight bash >}}
&nbsp;
group_vars/all
&nbsp;
{{< /highlight >}}

With this format:

{{< highlight bash >}}
&nbsp;
variable: value
&nbsp;
{{< /highlight >}}
