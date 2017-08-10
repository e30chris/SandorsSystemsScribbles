---
title: "Run Ansible through pmksh"
date: "2014-09-03"
tags: [ "ansible", "bullshit", "techdebt", "oldcorp" ]
categories: [ "ansible" ]
image: ""
---

## The problem
Old corporations use shitty old software to do things that modern Linux is really good at doing itself.  Using pmksh instead of a well laid out sudoers setup is what the problem is.  


## The goal
Configure Ansible playbooks to execute through the pmksh shell on the targeted servers.



## Shells & Ansible
Currently Ansible & Python work together to take the output from bash and report back with formatting.  Ansible does not understand ksh talk making running through pmksh a workaround for now.  Once Ansible add ksh to its list of shells then the current set of playbooks that are using 'command' can move to using the full suite of modules.  

The AIX setup I am Ansiblizing now using bash shell for standard user commands and pmksh for all root level (yes it should be sudoers) commands.  Tasks that do not need root level permissions I am using standard modules.  Tasks that need root I am using the following HowTo.

Ansible Shells:

[github repo - ansible/ansible/tree/devel/lib/ansible/runner/shell_plugins](https://github.com/ansible/ansible/tree/devel/lib/ansible/runner/shell_plugins)

## HowTo

Current Workaround:


{{< highlight bash >}}
&nbsp;
sandor@pineapplez:$ ansible -i hosts all -m command -a "/opt/quest/bin/pmksh -c '/bin/touch /root/bmw.325'"
&nbsp;
{{< /highlight >}}

{{< highlight bash >}}
&nbsp;
tasks:

- name: run a command through pmksh
  command: "/opt/quest/bin/pmksh -c /bin/touch /root/bmw.325'"
&nbsp;
{{< /highlight >}}

_once ksh is added to the list of shells I will update this post_
