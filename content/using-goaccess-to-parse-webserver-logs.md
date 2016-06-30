+++
title = ""
date = "2013-10-23"
tags = [ "", "" ]
categories = [ "" ]
image = "MIWG_2013_Kirkland_Concours_14.jpg"
+++

# The Problem

Need the ability to track web traffic for a specific hours worth of logs.  

# The Solution

Using GoAccess to parse Apache logs that are rotated into hour chunks.  

# The Goal

Install GoAccess to parse webserver logs.  Use Ansible Playbooks for the installation, configuration and for re-usabilty of GoAccess on other servers.  Keep it simple by rotating logs on the hour.




# The Links

- GoAccess - [real-time web log analyzer - allinurl/goaccess](https://github.com/allinurl/goaccess/)
- Ansible - [Ansible Docs](http://docs.ansible.com/)
- Apache rotatelogs - [Hourly Rotated WebServer Logs](https://httpd.apache.org/docs/2.2/programs/rotatelogs.html)
- Digital Ocean - [My referral link, thanks!](https://www.digitalocean.com/?refcode=980586449ebd)
- TugBoat - [CLI for DO - pearkes/tugboat](https://github.com/pearkes/tugboat)


# Ansible Playbook - Install GoAccess

Create the GoAccess Playbook.  This installs GoAccess using Apt on Debian.


<script src="https://gist.github.com/e30chris/9929303.js"></script>


Run the playbook with Ansible.

~~~
sandor@pineapplez:$ ansible-playbook ~/playbooks/goaccess/goaccess.yml -vvvv
~~~


# GoAccess Usage

Now that GoAccess is installed on the server we can login and parse the webserver logs.


~~~
sandor@argo:/var/log/apache2$ goaccess -f apache2.log
~~~

GoAccess now parses the log and outputs the results in a configurable format.  You can also export the results into various file formats.

Here is a screenshot that shows how many times an IP has hit the webserver.

![goscreen](https://s3.amazonaws.com/sandorssystemsscribbles/GoAccessScreen.png)


To keep it simple install Apache rotatelog and set the logs to rollover on the hour.  Then GoAccess can parse the logs by the hour.


Apache rotatelog setting for hourly logs:

~~~
CustomLog "|bin/rotatelogs -l /var/log/apache2.%Y-%m-%d-%H 3600" common
~~~

Parsing a specific hours logs with GoAccess

Example - Logs for 8am April, 2 2014.

~~~
sandor@argo:/var/log/apache2$ goaccess -f apache2.2014-04-02-08
~~~
