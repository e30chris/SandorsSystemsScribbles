---
title: "Add Twitter Timeline to OctoPress Sidebar"
date: "2013-11-26"
tags: [ "webstuff", "octopress" ]
categories: [ "webstuff" ]
image: ""
---

## The Goal
- Add a Twitter timeline to the sidebar of an Octopress blog



## The links

Octopress - [link](http://octopress.org/)

Acqui-hire.me - [link](http://acqui-hire.me/blog/2012/11/07/octopress-and-twitter-timelines/)

## Create a Twitter widget

Goto the Twitter widget settings page [here](https://twitter.com/settings/widgets) and create your timeline.

## Twitter Aside

Add a twitter html page to `source/_include/custom/asides`

{% gist 7669257 %}

Add that aside in your _config.yml

{{< highlight bash >}}
# list each of the sidebar modules you want to include, in the order you want them to appear.
# To add custom asides, create files in /source/_includes/custom/asides/ and add them to the list like 'custom/asides/custom_aside_name.html'
default_asides: [asides/recent_posts.html, asides/category_list.html, asides/twitter.html, asides/github.html, asides/googleplus.html]
{{< highlight bash >}}

and update the Twitter settings

{{< highlight bash >}}
# Twitter Embed Timeline
twitter_user: tweety_username
twitter_widget_id: widget_id_from_twitter_widget_code_blob
twitter_tweet_button: true
{{< highlight bash >}}

## rake generate | deploy

{{< highlight bash >}}
&nbsp;
spudBud@pineApplez> rake generate
spudBud@pineApplez> rake deploy
&nbsp;
{{< /highlight >}}

## The results
You should now have your Twitter timeline embedded on the sidebar of your OctoPress blog.
