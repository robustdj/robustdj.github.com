---
layout: post
title: "Using .rvmrc"
date: 2010-12-06 23:27
comments: true
categories: 
  - rails
  - ruby
  - rvm
---

If you’re not already using [RVM](https://rvm.io/), I’d highly recommend it as it makes it easier to manage different versions of ruby. You can
have different projects running on different versions of ruby and switch rubies with ease. One way to make this easier is to
make use of the .rvmrc file. It will automatically switch to the ruby version that your project is running on when you cd
into it.

Suppose we have a rails project called donut running on ruby 1.9.2.

```
cd donut
```

Once in the rails root of the project, create a file called `.rvmrc` and add this:

```
rvm use ruby-1.9.2-p0@donut --create
```

To list known rubies you can use this:

```
rvm list known
```

Now when you cd into the directory, it will automatically use ruby 1.9.2.

```
cd ..
cd donut

info: Using ruby 1.9.2 p0 with gemset donut
```

Notice that it is using the gemset donut. Gemsets are basically compartmentalized ruby setups. Gems installed in this gemset
will be separated from other projects (assuming they use different gemsets). You can read more about it
[here](https://rvm.io/gemsets/)

