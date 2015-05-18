---
layout: post
title: "post to octopress and deploy to github pages"
date: 2013-11-12 21:06
comments: false
summary: The Goal - Use Jekyll & octopress to post to Sandors Systems Scribbles which is hosted on GitHub Pages
categories: webstuff
---

## The Goal
Use Jekyll & octopress to post to Sandors Systems Scribbles which is hosted on GitHub Pages (where you are [now](http://sandorsscribbl.es/post-to-octopress-and-deploy-to-github-pages/)) 

_not anymore, Sandors HowTo notebook has been moved to Amazon AWS_



## The Links

- Jekyll - [link](http://jekyllrb.com/)

- OctoPress - [link](http://octopress.org)

- GitHub Pages - [link](http://pages.github.com)


## Create a new post

~~~
pineApplez>e30chris.github.io $rake new_post
Enter a title for your post: post to octopress and deploy to github pages
mkdir -p source/_posts
Creating new post: source/_posts/2013-11-12-post-to-octopress-and-deploy-to-github-pages.markdown
~~~
Write the post in markdown, save it.  [Mou](http://mouapp.com/) is a nice markdown editor for OS X.

## Generate new blog
This throws away everything and recreates the static files from scratch.

~~~
pineApplez>e30chris.github.io $rake -v generate
## Generating Site with Jekyll
identical source/stylesheets/screen.css
Configuration from /Users/Sandors/Codestuff/e30chris.github.io/_config.yml
Building site: source -> public
Successfully generated site: source -> public
~~~

## Deploy new blog
This does a bunch of stuff to move the new blog content in the 'source' branch to the 'master' branch on GitHub which then updates the GitHub Pages content.

~~~
pineApplez>e30chris.github.io $rake -v deploy
## Deploying branch to Github Pages
## Pulling any updates from Github Pages
cd _deploy
remote: Counting objects: 7, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 5 (delta 2), reused 5 (delta 2)
Unpacking objects: 100% (5/5), done.
From github.com:e30chris/e30chris.github.io
   d6ed173..281b573  source     -> origin/source
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream master origin/<branch>

cd -
rm -rf _deploy/CNAME
rm -rf _deploy/assets
rm -rf _deploy/atom.xml
rm -rf _deploy/blog
rm -rf _deploy/categories
rm -rf _deploy/favicon.png
rm -rf _deploy/images
rm -rf _deploy/index.html
rm -rf _deploy/javascripts
rm -rf _deploy/robots.txt
rm -rf _deploy/sitemap.xml
rm -rf _deploy/stylesheets
rm -rf _deploy/vanilla-vagrant-setup

## Copying public to _deploy
cp -r public/. _deploy
cd _deploy

## Commiting: Site updated at 2013-11-13 05:02:52 UTC
[master 3df53cc] Site updated at 2013-11-13 05:02:52 UTC
 8 files changed, 484 insertions(+), 8 deletions(-)
 create mode 100644 auto-update-virtualbox-guest-additions-with-vagrant-vbguest/index.html

## Pushing generated _deploy website
Counting objects: 29, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (11/11), done.
Writing objects: 100% (16/16), 5.66 KiB, done.
Total 16 (delta 8), reused 0 (delta 0)
To git@github.com:e30chris/e30chris.github.io.git
   02467a6..3df53cc  master -> master

## Github Pages deploy complete
cd -
~~~

### Commit the new post
Now that the code is up on GitHub save it in the git repo.

~~~
pineApplez>e30chris.github.io $git add .
pineApplez>e30chris.github.io $git commit -m 'post - post to octopress and deploy to github pages'
[source e200e58] post - post to octopress and deploy to github pages
 1 file changed, 52 insertions(+)
 create mode 100644 source/_posts/2013-11-12-post-to-octopress-and-deploy-to-github-pages.markdown
pineApplez>e30chris.github.io $git push origin source
~~~

Now write more scribbles and make the nerd world even more lazyier.
