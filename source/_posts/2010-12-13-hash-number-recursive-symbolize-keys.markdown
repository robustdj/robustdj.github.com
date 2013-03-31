---
layout: post
title: "Hash#recursive_symbolize_keys"
date: 2010-12-13 23:38
comments: true
categories: 
  - rails
  - ruby
---

Surprisingly there isn’t a `Hash#recursive_symbolize_keys` method so here’s code to accomplish that:

```ruby
class Hash
  def recursive_symbolize_keys
    symbolize_keys!
    values.select{|v| v.is_a? Hash}.each{|h| h.recursive_symbolize_keys}
    self
  end
end
```

Sometimes gems/plugins only take arguments in the form of symbols so this can method can be handy if you have some
sort of nested config hash.

Credits to: [http://grosser.it/2009/04/14/recursive-symbolize_keys/](http://grosser.it/2009/04/14/recursive-symbolize_keys/)


