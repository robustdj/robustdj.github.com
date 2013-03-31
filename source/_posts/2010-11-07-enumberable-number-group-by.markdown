---
layout: post
title: "Enumberable#group_by"
date: 2010-11-07 23:09
comments: true
categories: 
  - ruby
---

So I have another boy on the way and I was trying to think up of names so I created a ruby script to pick a
random name out of a file beginning with a letter that you provide as an argument. So you'd call the script like this:

```bash
ruby boy_name.rb s
=> Sylen
```

```ruby
# using ruby 1.8.7
 
if ARGV.empty?
  puts "USAGE: ruby boy_name.rb <letter>"
  exit
end
 
file = File.open("boy_names.txt")
names = file.lines.to_a.group_by{|name| name[0,1]}
selected_names = names[ARGV[0].capitalize]
puts selected_names[rand(selected_names.size)]
```

What's cool about it is this line

```ruby
names = file.lines.to_a.group_by{|name| name[0,1]}
```

`Enumberable#group_by` will group elements in an array by the result of the block passed in. In this case, we're grouping by
the first letter.

```ruby
%w{apple banana cucumber}.group_by{|fruit| fruit[0,1]}
=> #<OrderedHash {"a"=>["apple"], "b"=>["banana"], "c"=>["cucumber"]}>
```

