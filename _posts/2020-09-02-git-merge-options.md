---
date: 2020-08-31 00:30:00 +0530
layout: post
title: GIT Merge Strategy Options
subtitle: GIT Merge Strategy Options with Examples
description: GIT is a lightweight Distributed Version Control System
image: /assets/img/Introduction-To-GIT.png
optimized_image: /assets/img/git-merge.gif
category: code
tags:
  - code
  - git
author: ankurmalviya1618
my_prop: 'nil'
---
<h3>Git Merge Strategies</h3>
<p>
    A merge happens when combining two branches. Git will take two (or more) commit pointers and attempt to find a common base commit between them. 
    Git has several different methods to find a base commit, these methods are called "merge strategies". <br>
    Once Git finds a common base commit it will create a new "merge commit" that combines the changes of the specified merge commits. Technically, a merge commit is a regular commit which just happens to have two parent commits.
</p>
<img src="/assets/img/what-is-a-merge.gif" alt="">

