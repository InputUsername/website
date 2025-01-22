+++
title = "How to edit someone else's pull request on GitHub"
date = 2025-01-17
description = "A convenient workflow for making changes to a pull request on your project"
draft = true
+++

A while ago I found a blog post by Sam Sycamore [^1] that nicely explained a convenient workflow for editing other people's pull requests. However, that blog seems to have disappeared and the Wayback Machine also doesn't have it, so I decided to do a writeup for future reference while I still remember how it works.

But first, some context. Say your project has received an awesome pull request to fix a long-standing bug. Nice! There are a few changes you'd like to make, but for the most part it's good to go. However, the author seems to have disappeared and they are not responding to your comments. Luckily, GitHub has a feature that lets maintainers modify pull requests on their repositories. The pull request author can disable this, but usually, it's enabled. With this feature, you as a maintainer can push commits to the PR's branch on the author's fork.

[^1]: Original post: [link]((https://tech.sycamore.garden/add-commit-push-contributor-branch-git-github))
