---
date: 2020-08-31 00:30:00 +0530
layout: post
title: GIT for Beginners
subtitle: Git - The Open Source Version Control System from Linus Torvalds.
description: GIT is a lightweight Distributed Version Control System
image: /assets/img/Introduction-To-GIT.png
optimized_image: /assets/img/Introduction-To-GIT.png
category: code
tags:
  - code
  - git
author: ankurmalviya1618
my_prop: 'nil'
---
<h1 style="text-align:center;"> 
What is GIT
</h1>
Git is a distributed revision control and source code management system with an emphasis on speed.

Git was initially designed and developed by Linus Torvalds for Linux kernel development.

<h3>Version Control System</h3>
<strong>Version Control System (VCS)</strong> is a software that helps software developers to work together and maintain a complete history of their work.
<ul>
    <li>Allows developers to work simultaneously.</li>
    <li>Does not allow overwriting each other’s changes.</li>
    <li>Maintains a history of every version.</li>
</ul>
There are two types of <strong>VCS</strong> :-
<ul>
    <li>Centralized version control system (CVCS).</li>
    <li>Distributed/Decentralized version control system (DVCS).</li>
</ul>

<h3>Advantages of Git</h3>
<ul>
    <li><i>Free and open source</i></li>
    Git is released under GPL’s open source license.
    <li><i>Fast and small</i></li>
    As most of the operations are performed locally, it gives a huge benefit in terms of speed. 
    Git does not rely on the central server; that is why, there is no need to interact with the remote server for every operation. 
    The core part of Git is written in C, which avoids runtime overheads associated with other high-level languages. 
    Though Git mirrors entire repository, the size of the data on the client side is small.
    <li><i>Implicit backup</i></li>
    <li><i>Security</i></li>
    Git uses a common cryptographic hash function called secure hash function (SHA1), to name and identify objects within its database. 
    Every file and commit is check-summed and retrieved by its checksum at the time of checkout.
    <li><i>No need of powerful hardware</i></li>
    In case of CVCS, the central server needs to be powerful enough to serve requests of the entire team.
    In case of DVCS, developers don’t interact with the server unless they need to push or pull changes.
    <li><i>Easier branching</i></li>
    CVCS uses cheap copy mechanism, If we create a new branch, it will copy all the codes to the new branch, so it is time-consuming and not efficient. 
    Also, deletion and merging of branches in CVCS is complicated and time-consuming. 
    But branch management with Git is very simple. It takes only a few seconds to create, delete, and merge branches.
</ul>

<h3>Basic Workflow of GIT</h3>
<strong>Step 1. </strong> Modify/Add/Delete a file in the working area.<br>
<strong>Step 2. </strong> Add these files to the staging area to be tracked(same as ade commit)<br>
<strong>Step 3. </strong> Commit these files in the local repository with a message.<br>
<strong>Step 4. </strong> Push these changes to remote repository (same as ade mergetrans)<br>

<img src="/assets/img/Git_basic_operations.png" alt="Git Basic Operations Image">

``` bash
# adds file to the staging area
[bash]$ git add sort.c

# First commit
[bash]$ git commit –m “Added sort operation”

# adds file to the staging area
[bash]$ git add search.c

# Second commit
[bash]$ git commit –m “Added search operation”
```

<h3>Git - Lifecycle</h3>
<strong>General workflow is as follows :: </strong>
<ul>
    <li>Clone the Git repository as a working copy.</li>
    <li>Modify the working copy by adding/editing files.</li>
    <li>If necessary, we can also update the working copy by taking other developer's changes.</li>
    <li>Review the changes before commit.</li>
    <li>Commit changes. If everything is fine, then you push the changes to the repository.</li>
    <li>After committing, if something is wrong, then you correct the last commit and push the changes to the repository.</li>
</ul>

