---
title: "Dogecoin mining on Digital Ocean"
date: "2014-03-12"
tags: [ "dogecoin", "coins", "digitaloceans" ]
categories: [ "digitaloceans" ]
image: "dogecoinsuchlogo.png"
---

# The Goal

Create a droplet on Digital Ocean to mine Dogecoins so fun much profits.

![doge](https://dl.dropboxusercontent.com/u/6735750/dogecoinsuchlogo.png)



# The Links

 - Dogecoin - [very link](http://dogecoin.com/)
 - /r/dogecoin - [so reddit](http://www.reddit.com/r/dogecoin)
 - /r/dogemining - [many mines](http://www.reddit.com/r/dogemining)
 - Dogecoin Mining Pools - [much swim](http://www.doktorrf.com/dogecoin/pools.html)
 - Dogecoin Resources - [sources](https://github.com/ummjackson/dogecoin-resources)
 - Dogecoin Foundation - [now found](http://foundation.dogecoin.com/)
 - Digital Ocean - [awesome drops](https://www.digitalocean.com)
 - TugBoat - [command line ocean](https://www.digitalocean.com/community/articles/how-to-use-tugboat-to-manage-digitalocean-droplets-from-a-terminal)



# Create a droplet

{{< highlight bash >}}
sandor@argo> tugboat create lucydoge -s 62 -i 308287 -r 3 -k 44888
{{< /highlight >}}


{{< highlight bash >}}
Name:             lucydoge
Status:           active
Region ID:        3
Image ID:         308287
Size ID:          62
Backups Active:   false

{{< /highlight >}}

## Create user

{{< highlight bash >}}
root@lucydoge:~# history
root@lucydoge:~# ls -al
root@lucydoge:~# useradd -m lucy
root@lucydoge:~# passwd lucy
root@lucydoge:~# adduser lucy sudo
&nbdp;
{{< /highlight >}}


## Update Debian

{{< highlight bash >}}
&nbdp;
root@lucydoge:~# apt-get -y update && apt-get -y upgrade
&nbdp;
{{< /highlight >}}

## Install Screen

{{< highlight bash >}}
&nbdp;
root@lucydoge:~# apt-get install screen
&nbdp;
{{< /highlight >}}

# Install a CPU Miner

{{< highlight bash >}}
&nbdp;
root@lucydoge:~# sudo apt-get install git build-essential autotools-dev libcurl4-gnutls-dev autoconf automake
&nbdp;
{{< /highlight >}}

{{< highlight bash >}}
&nbdp;
lucy@lucydoge:~# mkdir ~/miner2049er
&nbdp;
{{< /highlight >}}

{{< highlight bash >}}
&nbdp;
lucy@lucydoge:~# cd miner2049er
lucy@lucydoge:~# git clone https://github.com/pooler/cpuminer.git
lucy@lucydoge:~# cd cpuminer
lucy@lucydoge:~# ./autogen.sh
lucy@lucydoge:~# CFLAGS="-O3 -Wall -msse2" ./configure
lucy@lucydoge:~# make
&nbdp;
{{< /highlight >}}

# Find a Mining Pool

[Mining Pools](http://www.doktorrf.com/dogecoin/pools.html)

 - Create a new address for mining deposits
 - Add that mining address to your pool profile


# Start mining

{{< highlight bash >}}
&nbdp;
lucy@lucydoge:~# ./minerd --url stratum+tcp://server:port --userpass worker.name:password
&nbdp;
{{< /highlight >}}

enjoy much coins
