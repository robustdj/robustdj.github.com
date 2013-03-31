---
layout: post
title: "Deleting Tags with Git"
date: 2010-11-06 23:07
comments: true
categories: 
---

You can list tags by running this:

```
git tag
```

Suppose we see a tag called v1.0.1 that we want to remove. We’d simply remove it like this:

```
git tag -d v1.0.1
git push origin :refs/tags/v1.0.1
```

The first line will remove the tag locally and the second line will remove the remote tag.

