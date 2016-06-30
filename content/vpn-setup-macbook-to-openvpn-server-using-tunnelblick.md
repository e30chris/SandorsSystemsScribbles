+++
title = "VPN Setup - MacBook to OpenVPN Server using Tunnelblick"
date = "2013-12-10"
tags = [ "security", "mac" ]
categories = [ "workstation" ]
image = ""
+++

## The Goal

- Get MacBook to connect to a OpenVPN Server
- Install Tunnelblick to make that happen



## Setup the OpenVPN Server

- Follow this - [sandorsscribbl.es/vpn-setup-openvpn-server-deploy-on-aws](http://sandorsscribbl.es/vpn-setup-openvpn-server-deploy-on-aws/)


## Install Tunnelblick

[Tunnelblick Google Code](https://code.google.com/p/tunnelblick/)


## Download OpenVPN Client Config file

- Goto OpenVPN web client
- Login with new user
- Choose to download cert


## Modify Client Config File for Tunnelblick

- Create a folder for the new config / VPN connection
- The $vpnname.tblk folder is where that VPN connection is saved

~~~
sandor@pineAplez> mkdir -p ~/Library/Application\ Support/Tunnelblick/Configurations/argovoyage.tblk/Contents/Resources/
~~~

- Copy the .ovpn client config file to that directory


~~~
sandor@pineAplez> cp ~/Downloads/client.ovpn ~/Library/Application\ Support/Tunnelblick/Configurations/argovoyage.tblk/Contents/Resources/.
~~~

## Start Tunnelblick

- Choose VPN from the tunnel


## The Test

- Verify IP using Tunnelblick
- Verify IP with Tunnelblick off
- [IP Chicken](http://ipchicken.com/)



## The Result

- Using a VPN your nets are safe from all prying eyes except the NSAs
