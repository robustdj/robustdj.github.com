---
layout: post
title: "Caching with Instance Variables"
date: 2010-11-17 23:13
comments: true
categories: 
---

A performance trick that I use a lot, especially in models, is memoizing methods that are expensive. Usually
these are methods that hit the database, make external API calls, involve longs loops, etc. Suppose we have a
model Song that has a genre column in the database and a method jazz that queries for all songs that have the
genre `jazz`. The model would look something like this:

```ruby
class Song < ActiveRecord::Base
  def jazz
    all(:conditions => {:genre => "jazz"})
  end
end
```

Now this looks all fine and dandy but what if that method gets called several times? It will cause a query every
time and will slow down performance.

##Memoization to the Rescue

We can trade space for time by saving the results in an instance variable. So the first time the method is called, a query to the database will be executed. However all subsequent calls to the method will not query the database and will just return the saved result. That would look like this:

```ruby
class Song < ActiveRecord::Base
  def jazz
    @jazz ||= all(:conditions => {:genre => "jazz"})
  end
end
```

##Clearing the cache!

A common problem with caching is forgetting to clear it. Whenever a new song gets added, deleted, or modified, we’re going to need to clear the `@jazz` instance variable so that when the jazz method is called, it will get those new songs. Easy enough, we can just add callbacks to clear the cache after saving. Check it out:

```ruby
class Song < ActiveRecord::Base
  after_save :clear_memoization
  after_destroy :clear_memoization

  def jazz
    @jazz ||= all(:conditions => {:genre => "jazz"})
  end        

  private
  def clear_memoization
    @jazz = nil
  end
end
```

