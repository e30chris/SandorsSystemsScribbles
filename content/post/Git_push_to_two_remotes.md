---
title: "Git push to two remotes"
date: "2017-08-10"
tags: [ "git", "github", "gitlab"]
categories: [ "git" ]
image: ""
---

## The Problem:
Need a way to share code hosted in a private GitLab repo publicly on GitHub.

## The solution:
Use a second Git remote to share a private Git repo publicly on GitHub.

### Backstory:
GitLab with its free unlimited private repository hosting and CiCd services offers a great place to host Git repos and build code.  GitHub is the defacto home of Open Source code repositories and is also a great way to add to a resume by sharing projects publicly.

---

### Pre-Requisites:
  - [GitLab](https://gitlab.com) account
  - [GitHub](https://github.com) account
  - Private Git repo hosted on GitLab
  - Public Git repo hosted on GitHub

### Verify the current remote

From the GitLab hosted repo:

{{< highlight bash >}}
&nbsp;
sandor@pineApplez$ git remote -v
origin	git@gitlab.com:e30chris/theargo.io.git (fetch)
origin	git@gitlab.com:e30chris/theargo.io.git (push)
&nbsp;
{{< /highlight >}}

### Add the second GitHub remote

{{< highlight bash >}}
&nbsp;
sandor@pineApplez$ git remote set-url --add --push origin git@github.com:e30chris/SandorsSystemsScribbles.git
&nbsp;
{{< /highlight >}}

### Verify the remotes

{{< highlight bash >}}
&nbsp;
sandor@pineApplez$ git remote -v
origin	git@gitlab.com:e30chris/theargo.io.git (fetch)
origin	git@github.com:e30chris/SandorsSystemsScribbles.git (push)
origin	git@gitlab.com:e30chris/theargo.io.git (push)
&nbsp;
{{< /highlight >}}

This is using `origin` as the remote name for both GitLab and GitHub.  Using `gitlab` and `github` as the remote names is another possibility and would enable pushing to GitHub only when the project is ready to be published publicly.
