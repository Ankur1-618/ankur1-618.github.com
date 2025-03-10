---
date: 2023-08-31 00:30:00 +0530
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
# What is GIT

Git is a distributed revision control and source code management system with an emphasis on speed. It was initially designed and developed by Linus Torvalds for Linux kernel development.

## Version Control System

**Version Control System (VCS)** is software that helps developers work together while maintaining a complete history of their work.

- Allows developers to work simultaneously.
- Prevents overwriting each other’s changes.
- Maintains a history of every version.

There are two types of **VCS**:
- Centralized version control system (CVCS).
- Distributed/Decentralized version control system (DVCS).

## Advantages of Git

- *Free and open source*  
  Git is released under GPL’s open source license.
- *Fast and small*  
  Most operations are performed locally, which makes Git fast. It doesn't depend on a central server for every operation, and its core is written in C to avoid runtime overhead. Even though Git mirrors the entire repository, the client-side data footprint is small.
- *Implicit backup*
- *Security*  
  Git uses the SHA1 cryptographic hash function to name and identify objects within its database. Every file and commit is checksummed, ensuring data integrity at checkout.
- *No need for powerful hardware*  
  In a CVCS, the central server must be powerful enough to serve all team requests. With DVCS, developers interact with the server only when pushing or pulling changes.
- *Easier branching*  
  In CVCS, creating a new branch involves copying all the code, which can be time-consuming and inefficient. In contrast, Git allows simple, quick branch creation, deletion, and merging.

## Basic Workflow of Git

1. **Step 1:** Modify/Add/Delete a file in the working area.
2. **Step 2:** Add these files to the staging area to be tracked.
3. **Step 3:** Commit these files in the local repository with a message.
4. **Step 4:** Push these changes to the remote repository.

![Git Basic Operations Image](/assets/img/Git_basic_operations.png)

```bash
# Adds file to the staging area
git add sort.c

# First commit
git commit -m "Added sort operation"

# Adds another file to the staging area
git add search.c

# Second commit
git commit -m "Added search operation"
```

## Git - Lifecycle

### General workflow is as follows:

- Clone the Git repository to create a working copy.
- Modify the working copy by adding or editing files.
- Update your working copy with changes from other developers if necessary.
- Review changes before committing.
- Commit your changes, and if everything is fine, push them to the repository.
- If needed, correct the last commit and push the revised changes to the repository.



