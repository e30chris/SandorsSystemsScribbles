---
title = "Configure new servers with the basics via Ansible"
date = "2015-07-08"
tags = [ "ansible", "infrastructure culture" ]
categories = [ "ansible" ]
image = ""
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

{{< highlight bash >}}
sandor@pineApplez$ tugboat create bloggindroplet -s 66 -i 10322623 -r 3 -k 915832
{{< / highlight >}}

That equals:

  -s size (66 = 512 mb)

  -i image (Centos 7 x64)

  -r region (San Francisco)

  -k ssh public key (mine)

### Add new droplet to SSH config

{{< highlight bash >}}
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
{{< / highlight >}}

### Add new droplet to Ansible hosts file

{{< highlight bash >}}
sandor@pineApplez$ › cat ~/.ansible/hosts
# All Servers
docks
cent7
fed22

# App servers
[app]

# DB servers
[db]

# Group 'multi' with all servers
[multi:children]

# Variables applied to all servers
[multi:vars]

{{< / highlight >}}


### Test ssh login

{{< highlight bash >}}
sandor@pineApplez$ ssh bloggindroplet
Last login: Thu Jul  9 00:34:41 2015 from 67.135.32.227
[root@bloggindroplet ~]#
{{< / highlight >}}

### Test Ansible

{{< highlight bash >}}
sandor@pineApplez$ ansible bloggindroplet -m ping
servfed | success >> {
    "changed": false,
    "ping": "pong"
}
sandor@pineApplez$
{{< / highlight >}}


### git clone the playbook

Download:

{{< highlight bash >}}
sandor@pineApplez$ git clone git@github.com:e30chris/Ansible-ServerDelivery.git ~/Codestuff/Ansible/.
{{< / highlight >}}

### Create the user accounts

via the variables in group_vars/main.yml

*delivered_users: this is the user accounts you want created on the new server, each with sudo access.*

{{< highlight yaml >}}
---
# vars file for ServerDelivery
delivered_users:
  - chrisl
  - dpr
  - weev
{{< / highlight >}}


### Give each users ssh key a password

#### and encrypt it with Ansible Vault

via group_vars/passes.yml

_this file will need to be created with ansible-vault_

{{< highlight bash >}}
sandor@pineApplez$ ansible-vault create group_vars/passes.yml
---
# vars file for ServerDelivery encrypted via ansible-vault
users_ssh_key_pass: stallman was right
{{< / highlight >}}

### Run the playbook

_This will run with 'hosts: all' which will put every server in your Ansible hosts file in this state.  This should be ok since all this playbook does is ensure sane security, adds users and updates packages.  Because Ansible is idempotent if these setting have not changed then nothing will be done.  If you do not want to run on all your hosts then specify that with either a group in the Ansible hosts file or with a cli switch._


{{< highlight bash >}}
sandor@pineApplez$ ansible-playbook site.yml -vv --ask-vault-pass
{{< / highlight >}}

which returns lots of cows:

{{< highlight bash >}}
2.2.3 in ServerDelivery/ on dev
› ansible-playbook -i "docks,cent7,fed22," site.yml --ask-vault-pass
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


ok: [fed22]
ok: [cent7]
ok: [docks]
docks: importing group_vars/Ubuntu.yml
cent7: importing group_vars/CentOS.yml
fed22: importing group_vars/Fedora.yml
 _____________________________________
< TASK: update all packages on CentOS >
 -------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


skipping: [docks]
skipping: [fed22]
skipping: [cent7]
 _______________________________
< TASK: install EPEL for CentOS >
 -------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


skipping: [cent7]
skipping: [fed22]
skipping: [docks]
 ________________________________________
< TASK: add the must have apps on CentOS >
 ----------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


skipping: [cent7]
skipping: [docks]
skipping: [fed22]
 ____________________________________________
< TASK: update all packages on Ubuntu family >
 --------------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


skipping: [cent7]
skipping: [fed22]
ok: [docks]
 ________________________________________
< TASK: add the must have apps on Ubuntu >
 ----------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


skipping: [cent7]
skipping: [fed22]
ok: [docks] => (item=sudo,vim,htop,tmux,unzip,fail2ban)
 _____________________________________
< TASK: update all packages on Fedora >
 -------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


skipping: [docks]
skipping: [cent7]
ok: [fed22]
 ________________________________________
< TASK: add the must have apps on Fedora >
 ----------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


skipping: [docks]
skipping: [cent7]
ok: [fed22] => (item=sudo,vim,htop,tmux,unzip,fail2ban)
 _______________________________
< TASK: adding users to servers >
 -------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


ok: [docks] => (item=chrisl)
ok: [fed22] => (item=chrisl)
ok: [cent7] => (item=chrisl)
ok: [docks] => (item=dpr)
ok: [fed22] => (item=dpr)
ok: [cent7] => (item=dpr)
ok: [cent7] => (item=weev)
ok: [fed22] => (item=weev)
ok: [docks] => (item=weev)
 _________________________________
< TASK: add users ssh public keys >
 ---------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


ok: [fed22] => (item=chrisl)
ok: [cent7] => (item=chrisl)
ok: [docks] => (item=chrisl)
ok: [fed22] => (item=dpr)
ok: [docks] => (item=dpr)
ok: [cent7] => (item=dpr)
ok: [fed22] => (item=weev)
ok: [docks] => (item=weev)
ok: [cent7] => (item=weev)
 _______________________
< TASK: add admin group >
 -----------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


ok: [docks]
ok: [fed22]
ok: [cent7]
 ________________________________
< TASK: add users to admin group >
 --------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


ok: [cent7] => (item=chrisl)
ok: [fed22] => (item=chrisl)
ok: [docks] => (item=chrisl)
ok: [cent7] => (item=dpr)
ok: [fed22] => (item=dpr)
ok: [docks] => (item=dpr)
ok: [cent7] => (item=weev)
ok: [fed22] => (item=weev)
ok: [docks] => (item=weev)
 ______________________________
< TASK: add users sudoers file >
 ------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


ok: [cent7] => (item=chrisl)
ok: [fed22] => (item=chrisl)
ok: [docks] => (item=chrisl)
ok: [cent7] => (item=dpr)
ok: [fed22] => (item=dpr)
ok: [docks] => (item=dpr)
ok: [cent7] => (item=weev)
ok: [fed22] => (item=weev)
ok: [docks] => (item=weev)
 __________________________________________
< TASK: secure SSH with a sane sshd_config >
 ------------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


ok: [docks]
ok: [cent7]
ok: [fed22]
 _______________________
< TASK: restarting sshd >
 -----------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


changed: [cent7]
changed: [fed22]
changed: [docks]
 ____________
< PLAY RECAP >
 ------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||


cent7                      : ok=8    changed=1    unreachable=0    failed=0
docks                      : ok=10   changed=1    unreachable=0    failed=0
fed22                      : ok=10   changed=1    unreachable=0    failed=0

{{< / highlight >}}
