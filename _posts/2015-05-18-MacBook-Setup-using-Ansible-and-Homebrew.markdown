---
layout: post
title: "MacBook Setup Using Ansible and Homebrew"
date: 2015-05-18 12:57
comments: false
summary: The problem: Have a fresh install of OSX and need to get the basics configured and installed like apps and sane OSX settings.
categories: ansible
---

## The Problem:
You have a brand new fresh install of OSX on your MacBook and you need to get up and running.  You want all your apps installed using Homebrew & Cask and OSX setup with goodies like zsh and .dotfiles.

## The solution:
Use Ansible to deploy CLI apps via Homebrew and GUI Apps via Cask.  Then have Ansible setup the shell with zsh and .dotfiles that are pulled from a GitHub repo.


### Backstory:
Setting up a new MacBook and trying to remember all the cool apps that were installed on the MacBook before that fast new SSD was installed was getting old.  Everything infrastructure is code according to proper DevOps and the MacBook that is the control center for that DevOps is certainly going to be code.

### ToDo:
  - Bring in OSX user settings like desktop backgrounds and other System Preferences.


---