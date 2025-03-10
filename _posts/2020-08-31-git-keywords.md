---
date: 2020-08-31 00:30:00 +0530
layout: post
title: GIT Keywords
subtitle: Git - Keywords and their uses/fuction
description: GIT is a lightweight Distributed Version Control System
image: /assets/img/Introduction-To-GIT.png
optimized_image: /assets/img/git_keywords.png
category: code
tags:
  - code
  - git
author: ankurmalviya1618
my_prop: 'nil'
---
<h3>Git Keywords</h3>

<h3>Blobs or Binary Large Object</h3>
<p>In GIT, each version of a file is represented by blob. A blob holds the file data but doesn’t contain any metadata about the file. It is a binary file, and in Git database, it is named as SHA1 hash of that file. In Git, files are not addressed by names. Everything is content-addressed.</p>

<h3>Trees</h3>
<p>Tree is an object, which represents a directory. It holds blobs as well as other sub-directories. A tree is a binary file that stores references to blobs and trees which are also named as SHA1 hash of the tree object.</p>

<h3>Commits</h3>
<p>Commit holds the current state of the repository. A commit is also named by SHA1 hash. You can consider a commit object as a node of the linked list. Every commit object has a pointer to the parent commit object. From a given commit, you can traverse back by looking at the parent pointer to view the history of the commit. If a commit has multiple parent commits, then that particular commit has been created by merging two branches.</p>

<h3>Branches</h3>
<p>Branches are used to create another line of development. By default, Git has a master (now main) branch. Usually, a branch is created to work on a new feature. Once the feature is completed, it is merged back with the master branch and we delete the branch. Every branch is referenced by HEAD, which points to the latest commit in the branch. Whenever you make a commit, HEAD is updated with the latest commit.</p>

<h3>Clone</h3>
<p>Clone operation creates the instance of the repository. Clone operation not only checks out the working copy, but it also mirrors the complete repository. Users can perform many operations with this local repository. The only time networking gets involved is when the repository instances are being synchronized.</p>

<h3>Pull</h3>
<p>Pull operation copies the changes from a remote repository instance to a local one. The pull operation is used for synchronization between two repository instances. This is same as the refresh operation in ADE.</p>

<h3>Push</h3>
<p>Push operation copies changes from a local repository instance to a remote one. This is used to store the changes permanently into the Git repository. This is same as the merge operation in ADE.</p>

<h3>HEAD</h3>
<p>HEAD is a pointer, which always points to the latest commit in the branch. Whenever you make a commit, HEAD is updated with the latest commit. The heads of the branches are stored in .git/refs/heads/ directory.</p>

