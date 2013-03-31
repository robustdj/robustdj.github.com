---
layout: post
title: "Deleting Branches with Git"
date: 2010-10-03 22:50
comments: true
categories: 
---

Suppose we have a remote and local branch called foo. To delete it locally, just run:

```
git branch -D foo
```

You can verify that it has been deleted by listing your local branches. To do that, run:

```
git branch
```

To delete the remote branch run:

```
git push origin :foo
```

To list all branches (including remotes) run:

```
git branch -a
```

