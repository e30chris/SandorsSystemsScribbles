---
layout: post
title: "Add Twitter Bootstrap 3 to OctoPress with OctoStrap3"
date: 2013-11-26 16:41
comments: false
summary: The Goal - Use awesome Twitter Bootstrap 3 on Octopress
categories: webstuff
---

## The Goal
- Use awesome Twitter Bootstrap 3 on Octopress
- Setup OctoStrap3 to do that



## The Links

OctoPress - [link](http://octopress.org)

OctoStrap3 - [link](http://kaworu.github.io/octopress/)


## Octopress Setup

Go [here](http://octopress.org/docs/setup/) and do all of that

## OctoStrap3 Setup

Instructions [here](http://kaworu.github.io/octopress/setup/install/)

From the base OctoPress dir

~~~
spudBud@pineApplez> git clone https://github.com/kAworu/octostrap3 .themes/octostrap3
spudBud@pineApplez> rake "install[octostrap3]"

~~~

## Category List Aside

Add a category list html page to `source/_includes/custom/asides`

{% gist 7669102 %}

Add the new aside in the _config.yml

~~~
# list each of the sidebar modules you want to include, in the order you want them to appear.
# To add custom asides, create files in /source/_includes/custom/asides/ and add them to the list like 'custom/asides/custom_aside_name.html'
default_asides: [asides/recent_posts.html, asides/category_list.html, asides/twitter.html, asides/github.html, asides/googleplus.html]
~~~

## rake generate | deploy

~~~ 
spudBud@pineApplez> rake generate
spudBud@pineApplez> rake deploy
~~~

You should now have a Twitter Bootstrapped UI on your OctoPress blog.  You should also have an aside that list each category, the number of posts for each and have a highlight if you are on that categories page.