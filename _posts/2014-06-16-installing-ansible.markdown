---
layout: post
title: "Installing Ansible"
date: 2014-06-16 13:15
comments: false
summary: The Problem - Need to deploy an Ansible Controller to run Playbooks from that uses the latest build and is easy to upgrade/configure.
categories: ansible
---

## The Problem
Need to deploy an Ansible Controller to run Playbooks from that uses the latest build and is easy to upgrade/configure.  

## The Solution
Install Ansible from the latest release on GitHub.

## The Goal
Create an Ansible Controller directory that will run the latest version via git clone and setup the shell with the Ansible environment scripts.



---

## Clone the Ansible repo
Go to the GitHub project page [github/ansible](https://github.com/ansible/ansible)

Choose a release version or the dev branch and clone.

~~~
sandor@pineapplez:$ mkdir ~/Codestuff/AnsibleController <-- Ansible runs from here
sandor@pineapplez:$ mkdir ~/Codestuff/ansibles <-- Playbooks go here
sandor@pineapplez:$ touch ~/Codestuff/ansibles/ansible_hosts <-- Server inventory goes here
sandor@pineapplez:$ cd ~/Codestuff/AnsibleController
sandor@pineapplez:$ git clone git@github.com:ansible/ansible.git
~~~

## Run the environment script

~~~
sandor@pineapplez:$ ./hacking/env-setup
~~~

To upgrade Ansible just go back into the AnsibleController directory and use git to get the latest.

You are now setup to run Ansible from the Controller.  Because the inventory file is inside the playbook dir or ~/Codestuff/ansibles/ansible_hosts it will not get overwritten if you change the AnsibleController dir.

Push some SSH public keys and start pushing playbooks!

