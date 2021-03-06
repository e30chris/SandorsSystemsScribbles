---
title: "MacBook Setup Using Ansible and Homebrew"
date: "2015-05-18"
tags: [ "ansible", "mac", "homebrew"]
categories: [ "ansible" ]
image: ""
---

## The Problem:
You have a brand new fresh install of OSX on your MacBook and you need to get up and running.  You want all your apps installed using Homebrew & Cask and OSX setup with goodies like zsh and .dotfiles.

## The solution:
Use Ansible to deploy CLI apps via Homebrew and GUI Apps via Cask.  Then have Ansible setup the shell with zsh and .dotfiles that are pulled from a GitHub repo.


[GitHub - e30chris/Ansible-MacDeploy](https://github.com/e30chris/Ansible-MacDeploy)


### Backstory:
Everything infrastructure is code according to proper DevOps and the MacBook that is the control center for that DevOps is certainly going to be setup using code as much as possible.

### ToDo:
  - Bring in OSX user settings like desktop backgrounds and other System Preferences.

---

### Pre-Requisites:
  - Fresh OSX install
  - Homebrew installed
  - Ansible installed via Homebrew
  - GitHub Authenticated
  - GitHub API token set
  - .dotfiles forked and customized to your liking


#### Fresh OSX Install

[USB install OSX](http://osxdaily.com/2014/10/16/make-os-x-yosemite-boot-install-drive/)

#### Homebrew installed

Follow the SysAdmin Bible and read the script before you run it:

[Github Raw of Homebrew install script](https://raw.githubusercontent.com/Homebrew/install/master/install)


Run the script once verified that you know what it will be doing:

{{< highlight bash >}}
&nbsp;
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
&nbsp;
{{< /highlight >}}

Verify Homebrew is correct:

{{< highlight bash >}}
&nbsp;
sandor@pineApplez$ brew doctor
&nbsp;
{{< /highlight >}}

#### Install Ansible via Homebrew

Installing the latest from dev:

{{< highlight bash >}}
&nbsp;
sandor@pineApplez$ brew install ansible --HEAD
&nbsp;
{{< /highlight >}}

#### GitHub Authenticated

{{< highlight bash >}}
&nbsp;
sandor@pineApplez$ git config --global user.name "sandor"
sandor@pineApplez$ git config --global user.email "chris@e30chris.me"
sandor@pineApplez$ ssh-keygen -t rsa -b 4096 -C "sandor@macbook"
sandor@pineApplez$ ssh-add ~/.ssh/id_rsa
sandor@pineApplez$ pbcopy < ~/.ssh/id_rsa.pub
&nbsp;
{{< /highlight >}}

Add your SSH Public Key to GitHub -> Settings.


#### GitHub API Token Set

This avoids annoying brew errors on lookups.

[stackoverflow](http://stackoverflow.com/questions/20130681/setting-github-api-token-for-homebrew#20130816)

#### .dotfile forked

[All hail Holmans .dotfiles](https://github.com/holman/dotfiles)

[Explained by Holman](http://zachholman.com/2010/08/dotfiles-are-meant-to-be-forked/)


### Download and run the playbook

Download:

{{< highlight bash >}}
&nbsp;sandor@pineApplez$ git clone git@github.com:e30chris/Ansible-MacDeploy.git ~/Codestuff/Ansible/.
&nbsp;
{{< /highlight >}}

Edit:

  - Set the OSX username that is running the playbook in vars/main.yml

Run:

{{< highlight bash >}}
&nbsp;
sandor@pineApplez$ ansible-playbook -i hosts site.yml --ask-sudo-pass
&nbsp;
{{< /highlight >}}

Then run the .dotfiles bootstrap script

{{< highlight bash >}}
&nbsp;
sandor@pineApplez$ ~/.dotfiles/script/bootstrap
&nbsp;
{{< /highlight >}}

Now go and setup all the OSX pieces that are not easily Ansibilized like the App Store.
