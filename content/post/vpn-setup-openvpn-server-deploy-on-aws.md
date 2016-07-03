---
title: "VPN Setup - OpenVPN Server Deploy on AWS"
date: "2013-12-10"
tags: [ "aws", "security" ]
categories: [ "aws" ]
image: ""
---

## The Goal
- Deploy an OpenVPN Server on AWS



## Launch the OpenVPN Access Server AMI

- Set AMI to VPC network
- Auto Assign Public Static IP

### AMI ids

| AMI Location       | ami id         
|:-------------|:-------------
|US East (Virginia)| ami-ff6b3096
|US West (Oregon)| ami-c8039bf8
|US West (N California)| ami-6c0b3d29
|EU West (Ireland)| ami-89d83afe
|Asia Pacific (Singapore)| ami-3c9bce6e
|Asia Pacific (Tokyo)| ami-172d4916
|Asia Pacific (Sydney)| ami-db73efe1
|South America (Sao Paulo)| ami-6d4ee870




### Set user-data

|Key|Value         
|:---|:----
|public_hostname    |hostname that clients should use to contact the server
|admin_user (default=openvpn)    |Access Server administrative account name
|license     |Access Server license key _without a license key only 2 connections allowed_
|reroute_gw default=0 |if 1, clients will route internet traffic through the VPN
|reroute_dns default=0 | if 1, clients will route DNS queries through the VPN


### Set New Security Group

|  Protocol    |  Type    |  Port    |  Source     |  Role
|:---------|:------------|:---------|:------------|:-----------
|SSH    |TCP    |22    |Anywhere 0.0.0.0/0| Linux Access
|HTTPS    |TCP    |443    |Anywhere 0.0.0.0/0| OpenVPN Client Web Server
|Custom TCP Rule     |TCP     |943    |Anywhere 0.0.0.0/0| OpenVPN Web UI
|Custom UDP Rule     |UDP    |1194    |Anywhere 0.0.0.0/0| Initiate UDP based VPN sessions



## Configure OpenVPN Server


~~~
spudBud@pineApplez> ovpn-init
~~~

## Access Client Website

https://"aws public ip":943/admin/

## Change default OpenVPN Admin Password


~~~
spudBud@pineApplez> passwd 'admin user set in user-data'
~~~

## Add a new VPN user


~~~
spudBud@pineApplez> useradd sandor
spudBud@pineApplez> passwd sandor
~~~


Login with new user account to web to download client config files.

https://"aws public ip":943/

## Configure VPN client


- MacBook - [sandorsscribbl.es/vpn-setup-macbook-to-openvpn-server-using-tunnelblick](http://sandorsscribbl.es/vpn-setup-macbook-to-openvpn-server-using-tunnelblick/)
- Fedora  - [sandorsscribbl.es/vpn-setup-fedora-to-openvpn-server](http://sandorsscribbl.es/vpn-setup-fedora-to-openvpn-server/)



## The result

A running OpenVPN Server that you can connect a MacBook or Linux box too.
