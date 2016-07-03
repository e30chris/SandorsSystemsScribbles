---
title: "Puppet vim setup"
date: "2013-11-12"
tags: [ "puppet", "vim" ]
categories: [ "workstation" ]
image: ""
---

## The Goal
Setup vim to edit Puppet Manifests using a few good tools that will get you to pass Puppet Lint tests.



## The Links

- PuppetLabs - [link](http://puppetlabs.com/)

- vim-puppet - [link](https://github.com/rodjek/vim-puppet)

- vim tabular - [link](https://github.com/godlygeek/tabular)

- vim syntastic - [link](https://github.com/scrooloose/syntastic)


## Vim-Pathogen
Vim-Pathogen on [github](https://github.com/tpope/vim-pathogen "vim-pathogen")

~~~
[root@Argon ~]# mkdir -p ~/.vim/autoload ~/.vim/bundle
[root@Argon ~]# cd ~/.vim/autoload
[root@Argon ~]# curl -o pathogen.vim https://raw.github.com/tpope/vim-pathogen/master/autoload/pathogen.vim
~~~
Add these settings to .vimrc

~~~
execute pathogen#infect()
syntax on
filetype plugin indent on
~~~

## Vim-Puppet
Vim-Puppet on [github](https://github.com/rodjek/vim-puppet "vim-puppet")

~~~
[root@Argon ~]# cd ~/.vim/bundle
[root@Argon ~]# git clone git://github.com/rodjek/vim-puppet.git
~~~

## Tabular
Tabular on [github](https://github.com/godlygeek/tabular "tabular")

~~~
[root@Argon ~]# cd ~/.vim/bundle
[root@Argon ~]# git clone git://github.com/godlygeek/tabular.git
~~~

## Syntastic
Syntastic on [github](https://github.com/scrooloose/syntastic "syntastic")

~~~
[root@Argon ~]# cd ~/.vim/bundle
[root@Argon ~]# git clone https://github.com/scrooloose/syntastic.git
~~~
