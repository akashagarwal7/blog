---
layout: post
title: How to enable SSH access to a Git server on a remote machine behind a firewall
# subtitle: Each post also has a subtitle
# gh-repo: akashagarwal7/blog
# gh-badge: [star, fork, follow]
tags: [ssh]
comments: true
---

Let’s consider a scenario -- you are deploying a software at an organisation’s development server (let’s call it Machine R). Depending upon the organisation, it is possible that firewall rules prohibit outgoing connections to port 22 (default for SSH) for obvious security reasons. In such a scenario, the usual option is to clone the required repositories over HTTPS. If the repositories are private, you would be required to enter your username and password every time you want to pull changes from remote. While doing so once or twice a week for a single repository may seek ok to many, it can quickly become tedious. Let’s see how you can remedy this!

SSH allows you to configure what port should be used to establish an SSH connection for a URL. The GitHub flavour for the same is covered at [this GitHub link](https://help.github.com/en/github/authenticating-to-github/using-ssh-over-the-https-port) in depth. If Machine R can access the internet over port 80 and 443, you are in luck since GitHub listens for SSH connections on port 443. For any other remote Git server to work, the SSH daemon on those servers need to listen to a port that can be communicated to from Machine R behind the firewall.

Most other commercial vendors like Gitlab premium also supports SSH over port 443 using a proxy, as described in  [their blog post](https://about.gitlab.com/blog/2016/02/18/gitlab-dot-com-now-supports-an-alternate-git-plus-ssh-port/).

----

## Footnotes

Git version >= `1.7.10` also allows password caching as explained [here](https://help.github.com/en/github/using-git/caching-your-github-password-in-git), but that might not be an option for/available to many.
