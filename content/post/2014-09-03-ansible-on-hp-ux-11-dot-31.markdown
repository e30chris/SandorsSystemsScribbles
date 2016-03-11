---
categories: ansible
comments: false
date: 2014-09-03T00:00:00Z
summary: The problem - Old corporations that are running ancient OS'es desperately
  need configuration management and automation to keep the dinosaurs efficient.
title: Ansible on HP-UX 11.31
url: /2014/09/03/ansible-on-hp-ux-11-dot-31/
---

## The problem
Old corporations that are running ancient OS'es desperately need configuration management and automation to keep the dinosaurs efficient.      


## The goal
Install Python on HP-UX so Ansible can manage and automate the ancient servers.


## HowTo

~~~
sandor@pineapplez:$ ansible -i hosts all -m raw -a "swinstall -x mount_all_filesystems=false -s depotserver:/depot/11.31/python python"
~~~