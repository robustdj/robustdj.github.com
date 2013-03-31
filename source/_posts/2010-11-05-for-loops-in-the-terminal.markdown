---
layout: post
title: "For Loops in the Terminal"
date: 2010-11-05 23:05
comments: true
categories: 
---

Recently I've had to retrieve information from a set of remote servers. Using the for loop in the terminal,
I was able to get all the information I needed in one line which was very convenient. I used the following
code snippet:

```
for i in 1 2 3 4 5 6 7 8; do echo "search$i"; ssh root@search$i 'ls -alhrt /ip/sphinx_index/production'; done
```
