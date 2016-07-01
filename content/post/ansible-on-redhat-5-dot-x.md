---
title = "Ansible on RedHat 5.x"
date = "2013-09-03"
tags = [ "ansible", "redhat", "linux" ]
categories = [ "linux" ]
image = "MIWG_2013_Kirkland_Concours_14.jpg"
---

## The problem
Older Pythons that live on older RedHats do not have json support.  Old corporations still use old OS'es like RedHat 5.x but need to use modern configuration management tools like Ansible.  


## The goal
Enable RedHat 5.x systems to run Ansible by installing json support on Python 2.4.


## HowTo

_Using Ansible Raw_

~~~
sandor@pineapplez:$ ansible -i hosts all -m raw -a "yum -y install python-simplejson"
~~~
