---
categories: ansible
comments: false
date: 2014-09-03T00:00:00Z
summary: The problem - Older Pythons that live on older RedHats do not have json support.  Old
  corporations still use old OS'es like RedHat 5.x but need to use modern configuration
  management tools like Ansible.
title: Ansible on RedHat 5.x
url: /2014/09/03/ansible-on-redhat-5-dot-x/
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