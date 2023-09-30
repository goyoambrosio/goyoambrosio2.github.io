---
title: "What is No Switch Yard (**NoSY**) methodology"
related : true
categories:
  - WhatIs
tags: 
  - Git
  - methodology
  - version
  - control
  - workflow
  - CERN
  - ROOT
---

As you know, source control is a system for recording the history of file
revisions allowing programmers to edit their code, safe in the knowledge that
they can always revert to a previous good state of code. Among a plethora of
alternatives Git is a distributed source control system created by Linus
Torvalds, to support the development of Linux. Git is an incredibly flexible
system which allows you to do pretty much anything.[¹]

"No Switch Yard" (**NoSY**) is a workflow for Git source control. It involves
creating branches from the master branch on which to develop new features and
regularly rebasing against the master branch so that when the time comes the
feature branch can be merged into the master branch via a pull request with
little fuss. We should not be producing a byzantine system by branching feature
branches from other feature branches. The aim of “No Switch Yard” is to make the
history as simple as possible and make merging branches back onto master as easy
as possible.[¹]

This workflow has the following advantages relative to always working on the master branch[²]:

- It is easy to keep track of upstream changes even when working on a protracted
  task.
- The change tree remains simple, easy to understand at a glance and even
  (mostly) linear (revision trees with multiple developers can quickly start
  looking like a train switch yard)
- Unsightly "merge with branch" commits are minimized.
- It is easy to keep separate unrelated tasks upon which you may be working
  simultaneously.
- Commits related to each other can be kept together or merged for increased
  clarity.

This workflow is heavely used with [ROOT](https://root.cern.ch/) projects at
[CERN](https://home.cern/). Root is a modular scientific software framework. It provides all the
functionalities needed to deal with big data processing, statistical analysis,
visualisation and storage. It is mainly written in C++ but integrated with other
languages such as Python and R.[³]

More info:

<https://scraperwiki.com/2013/10/git/>
<https://cdcvs.fnal.gov/redmine/projects/cet-is-public/wiki/GitTipsAndTricks>
<https://root.cern.ch/suggested-work-flow-distributed-projects-nosy>

[¹]: <https://scraperwiki.com/2013/10/git/>
[²]: <https://cdcvs.fnal.gov/redmine/projects/cet-is-public/wiki/GitTipsAndTricks>
[³]: <https://root.cern.ch/suggested-work-flow-distributed-projects-nosy>


