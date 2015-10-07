---
layout: post
title: "Configure new servers with the basics via Ansible"
date: 2015-07-08 12:51
comments: false
summary: The problem - Have a new droplet on Digital Ocean that you need just for the next two hours to try something, it needs to be configured with the basics like security, users and updated packages.
categories: ansible
---

## The Problem:
You have a throw away server that you only need for the next couple of hours to try something out.  You need the server configured so that you can login and get to work without messing with yum updates or adding your user and public ssh keys to access it.

## The solution:
This Ansible playbook that is simple and to the point.  It updates yum or apt, applies a sane sshd config, adds users and groups and then gets your public ssh key onto the throwaway box so you can login without a password.


[GitHub - e30chris/ServerDelivery](https://github.com/e30chris/Ansible-ServerDelivery)


### Backstory:
After pitching Ansible as a great solution to infrastructure automation I wanted a simple playbook to demo that would show off the simplest side of Ansible.  I also need the short lived servers I spin up on Digital Ocean to be secured and easy to login to without any manual steps.  This playbook takes care of both requirements.

### ToDo:
  - Add a role to this playbook to kick off the droplet creation and then register the names and IP's for the new servers before handing off to this current playbook config.

---

### Pre-Requisites
  - Servers booted and running
  - SSH access
  - SSH public keys for each user in files/
  - Server OS is in the Debian family or the RedHat family.


### Create the new droplets

Using the excellent Digital Ocean cli tool - [TugBoat - GitHub](https://github.com/pearkes/tugboat)

{% highlight bash %}
sandor@pineApplez$ tugboat create bloggindroplet -s 66 -i 10322623 -r 3 -k 915832
{% endhighlight %}

That equals:

  -s size (66 = 512 mb)
  
  -i image (Centos 7 x64)
  
  -r region (San Francisco)
  
  -k ssh public key (mine)

### Add new droplet to SSH config

{% highlight bash %}
sandor@pineApplez$ cat ~/.ssh/config
#SSH Stuff
Host bloggindroplet
  Hostname 45.55.7.178
  User root
Host *
  ServerAliveInterval 60
  ServerAliveCountMax 30
  ControlMaster auto
  ControlPath ~/.ssh/connections/ssh-%r@%h:%p
  ControlPersist 4h
  StrictHostKeyChecking no
  IdentityFile ~/.ssh/id_rsa
{% endhighlight %}

### Add new droplet to Ansible hosts file

{% highlight bash %}
sandor@pineApplez$ › cat ~/.ansible/hosts
# All Servers
bloggindroplet
servcent
servdeb
servfed

# App servers
[app]

# DB servers
[db]

# Group 'multi' with all servers
[multi:children]

# Variables applied to all servers
[multi:vars]

{% endhighlight %}


### Test ssh login

{% highlight bash %}
sandor@pineApplez$ ssh bloggindroplet
Last login: Thu Jul  9 00:34:41 2015 from 67.135.32.227
[root@bloggindroplet ~]#
{% endhighlight %}

### Test Ansible

{% highlight bash %}
sandor@pineApplez$ ansible bloggindroplet -m ping
servfed | success >> {
    "changed": false,
    "ping": "pong"
}
sandor@pineApplez$ 
{% endhighlight %}


### git clone the playbook

Download:

{% highlight bash %}
sandor@pineApplez$ git clone git@github.com:e30chris/Ansible-ServerDelivery.git ~/Codestuff/Ansible/.
{% endhighlight %}

### Create the user accounts

via the variables in group_vars/main.yml

*delivered_users: this is the user accounts you want created on the new server, each with sudo access.*

{% highlight yaml %}
---
# vars file for ServerDelivery
delivered_users:
  - chrisl
  - dpr
  - weev
{% endhighlight %}


### Give each users ssh key a password

#### and encrypt it with Ansible Vault

via group_vars/passes.yml

_this file will need to be created with ansible-vault_

{% highlight bash %}
sandor@pineApplez$ ansible-vault create group_vars/passes.yml
---
# vars file for ServerDelivery encrypted via ansible-vault
users_ssh_key_pass: stallman was right
{% endhighlight %}

### Run the playbook

_This will run with 'hosts: all' which will put every server in your Ansible hosts file in this state.  This should be ok since all this playbook does is ensure sane security, adds users and updates packages.  Because Ansible is idempotent if these setting have not changed then nothing will be done.  If you do not want to run on all your hosts then specify that with either a group in the Ansible hosts file or with a cli switch._


{% highlight bash %}
sandor@pineApplez$ ansible-playbook site.yml -vv --ask-vault-pass
{% endhighlight %}

which returns lots of cows:

{% highlight bash %}
sandor@pineApplez$ ansible-playbook site.yml --ask-vault-pass
Vault password:
 ____________
< PLAY [all] >
 ------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


 _________________
< GATHERING FACTS >
 -----------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


ok: [servdeb]
ok: [servfed]
ok: [servcent]
 ___________________________________________________
< TASK: update all YUM packages to latest on Centos >
 ---------------------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


skipping: [servdeb]
skipping: [servfed]
ok: [servcent]
 ___________________________________________________
< TASK: update all DNF packages to latest on Fedora >
 ---------------------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


skipping: [servcent]
skipping: [servdeb]
ok: [servfed]
 ___________________________________________________
< TASK: update all Apt packages to latest on Debian >
 ---------------------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


skipping: [servcent]
skipping: [servfed]
ok: [servdeb]
 ___________________________________________________
< TASK: add sudoers package to Debian for new users >
 ---------------------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


skipping: [servfed]
skipping: [servcent]
ok: [servdeb]
 _______________________________
< TASK: adding users to servers >
 -------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


ok: [servcent] => (item=chrisl)
ok: [servdeb] => (item=chrisl)
ok: [servfed] => (item=chrisl)
ok: [servcent] => (item=copper)
ok: [servfed] => (item=copper)
ok: [servdeb] => (item=copper)
ok: [servcent] => (item=lucy)
ok: [servdeb] => (item=lucy)
ok: [servfed] => (item=lucy)
ok: [servfed] => (item=kami)
ok: [servdeb] => (item=kami)
ok: [servcent] => (item=kami)
ok: [servfed] => (item=bimmer)
ok: [servcent] => (item=bimmer)
ok: [servdeb] => (item=bimmer)
 _________________________________
< TASK: add users ssh public keys >
 ---------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


ok: [servfed] => (item=chrisl)
ok: [servdeb] => (item=chrisl)
ok: [servcent] => (item=chrisl)
ok: [servfed] => (item=copper)
ok: [servdeb] => (item=copper)
ok: [servcent] => (item=copper)
ok: [servfed] => (item=lucy)
ok: [servdeb] => (item=lucy)
ok: [servcent] => (item=lucy)
ok: [servfed] => (item=kami)
ok: [servdeb] => (item=kami)
ok: [servcent] => (item=kami)
ok: [servdeb] => (item=bimmer)
ok: [servfed] => (item=bimmer)
ok: [servcent] => (item=bimmer)
 _______________________
< TASK: add admin group >
 -----------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


ok: [servcent]
ok: [servdeb]
ok: [servfed]
 ________________________________
< TASK: add users to admin group >
 --------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


ok: [servdeb] => (item=chrisl)
ok: [servcent] => (item=chrisl)
ok: [servfed] => (item=chrisl)
ok: [servdeb] => (item=copper)
ok: [servdeb] => (item=lucy)
ok: [servcent] => (item=copper)
ok: [servfed] => (item=copper)
ok: [servdeb] => (item=kami)
ok: [servfed] => (item=lucy)
ok: [servcent] => (item=lucy)
ok: [servdeb] => (item=bimmer)
ok: [servcent] => (item=kami)
ok: [servfed] => (item=kami)
ok: [servcent] => (item=bimmer)
ok: [servfed] => (item=bimmer)
 ______________________________
< TASK: add users sudoers file >
 ------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


ok: [servdeb] => (item=chrisl)
ok: [servfed] => (item=chrisl)
ok: [servcent] => (item=chrisl)
ok: [servdeb] => (item=copper)
ok: [servfed] => (item=copper)
ok: [servcent] => (item=copper)
ok: [servdeb] => (item=lucy)
ok: [servfed] => (item=lucy)
ok: [servcent] => (item=lucy)
ok: [servdeb] => (item=kami)
ok: [servcent] => (item=kami)
ok: [servfed] => (item=kami)
ok: [servdeb] => (item=bimmer)
ok: [servcent] => (item=bimmer)
ok: [servfed] => (item=bimmer)
 ____________________________________________________
< TASK: secure SSH with a sane sshd_config on RedHat >
 ----------------------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


skipping: [servdeb]
ok: [servfed]
ok: [servcent]
 ____________________________________________________
< TASK: secure SSH with a sane sshd_config on Debian >
 ----------------------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


skipping: [servfed]
skipping: [servcent]
ok: [servdeb]
 ____________________________________
< TASK: restart sshd with new config >
 ------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


changed: [servfed]
changed: [servdeb]
changed: [servcent]
 ____________
< PLAY RECAP >
 ------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


servcent                   : ok=9    changed=1    unreachable=0    failed=0
servdeb                    : ok=10   changed=1    unreachable=0    failed=0
servfed                    : ok=9    changed=1    unreachable=0    failed=0



{% endhighlight %}
