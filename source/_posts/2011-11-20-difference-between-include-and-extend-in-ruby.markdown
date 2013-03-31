---
layout: post
title: "Difference Between Include and Extend in Ruby"
date: 2010-11-20 23:17
comments: true
categories: 
  - ruby
---

A common thing to do in ruby is to share functionality among classes. Since ruby doesn’t support multiple inheritance, we
simply import code by mixing in modules. So instead of classes having a crazy inheritance tree, they can have includes
and/or extends at the top of the class. But what is the difference between include and extend?

They both mix in functionality into a class, but they are added to different places. Include mixes functionality into
instances of a class, whereas extend mixes functionality into a class itself.

Take a look at the following example:

```ruby
module Explosion
  def explode
    puts "BOOM!"
  end
end

class Dynamite
  include Explosion
end

Dynamite.new.explode # => BOOM!
Dynamite.explode # => NoMethodError: undefined method `explode'

class Bomb
  extend Explosion
end

Bomb.new.explode # => NoMethodError: undefined method `explode'
Bomb.explode # => BOOM!
```

And of course with ruby being dynamic and all, you can include and/or extend modules at run time

```ruby
class Chemical;end;
Chemical.extend Explosion
Chemical.explode # => BOOM!

# Class#include method is private so we have to use a workaround
Chemical.include Explosion 
# => NoMethodError: private method `include' called

Chemical.send(:include, Explosion)
Chemical.new.explode # => BOOM!
```

If you want more information on include and extend check out the following links:

* [Include vs Extend in Ruby by John Nunemaker](http://www.railstips.org/blog/archives/2009/05/15/include-vs-extend-in-ruby/)
* [Include vs Extend by Ruby Quicktips](http://rubyquicktips.com/post/1133877859/include-vs-extend)

