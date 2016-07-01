---
title = "Ansible on HP-UX 11.31"
date = "2014-09-03"
tags = [ "ansible", "hp-ux", "techdebt", "oldcorp" ]
categories = [ "ansible" ]
image = ""
---

## The problem
Old corporations that are running ancient OS'es desperately need configuration management and automation to keep the dinosaurs efficient.      


## The goal
Install Python on HP-UX so Ansible can manage and automate the ancient servers.


## HowTo

~~~
sandor@pineapplez:$ ansible -i hosts all -m raw -a "swinstall -x mount_all_filesystems=false -s depotserver:/depot/11.31/python python"
~~~
