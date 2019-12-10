---
layout: post
title: "Manage Home Directory"
date: 2019-12-10 12:07:00 +0800
categories: jekyll update
---

We share GPU box and we use anaconda to manage our Python environments these days. Then sooner or
later we'll encounter the issue that the home directory of the box is full. In this case, we can
use the following procedure to solve the problem

```bash
cd ~
# Find out which file or directory uses the most space
du -s ./*
du -s ./.* # check hidden files
# move the biggest directory to another location, such as .canda here
mv .canda /data/yanq/
# then link it back
ln -s /data/yanq/.conda .

```
