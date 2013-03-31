---
layout: post
title: "Loading and Dumping a MySQL Database"
date: 2010-12-05 23:25
comments: true
categories: 
---

You can dump the database into a file using:

```
mysqldump -h hostname -u user --password=password databasename > filename
```

You can load the dump into the database using:

```
mysql -h hostname -u user --password=password databasename < filename
```

