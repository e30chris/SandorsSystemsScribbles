+++
title = ""
date = "2013-10-23"
tags = [ "", "" ]
categories = [ "" ]
image = "MIWG_2013_Kirkland_Concours_14.jpg"
+++

## The problem
Old corporations that are running ancient OS'es desperately need configuration management and automation to keep the dinosaurs efficient.      


## The goal
Install Python on HP-UX so Ansible can manage and automate the ancient servers.


## HowTo

~~~
sandor@pineapplez:$ ansible -i hosts all -m raw -a "swinstall -x mount_all_filesystems=false -s depotserver:/depot/11.31/python python"
~~~
