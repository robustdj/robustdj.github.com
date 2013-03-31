---
layout: post
title: "RSpec Formatter Documentation with instant stack traces upon failure"
date: 2012-02-05 01:49
comments: true
categories: 
  - rspec
  - ruby
---

The documentation rspec formatter is nice to read, but the bad thing about it is that when a failure occurs,
it doesn't show a stack trace until the end of the test run. If you have a long running test suite, you might want to look
into the first few failures you encounter while the test suite finishes running.

With this simple formatter, it accomplishes exactly that.

{% gist 2571615 %}

Now when the test suite runs, the stack trace gets printed right away upon failures.

```
$ bundle exec rspec spec/models/player_spec.rb

Player
  #next
    in the main stream
      should return the next song
      should decrement play count if the song was skipped
      playing the first song
        should fetch and return the first song (FAILED - 1)
  1) Player#next in the main stream playing the first song should fetch and return the first song
     Failure/Error: fail
     RuntimeError:
     # ./spec/models/player_spec.rb:24:in `block (5 levels) in <top (required)>'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/example.rb:48:in `instance_eval'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/example.rb:48:in `block in run'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/example.rb:107:in `with_around_hooks'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/example.rb:45:in `run'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/example_group.rb:294:in `block in run_examples'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/example_group.rb:290:in `map'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/example_group.rb:290:in `run_examples'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/example_group.rb:262:in `run'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/example_group.rb:263:in `block in run'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/example_group.rb:263:in `map'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/example_group.rb:263:in `run'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/example_group.rb:263:in `block in run'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/example_group.rb:263:in `map'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/example_group.rb:263:in `run'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/example_group.rb:263:in `block in run'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/example_group.rb:263:in `map'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/example_group.rb:263:in `run'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/command_line.rb:24:in `block (2 levels) in run'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/command_line.rb:24:in `map'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/command_line.rb:24:in `block in run'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/reporter.rb:12:in `report'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/command_line.rb:21:in `run'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/runner.rb:80:in `run_in_process'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/runner.rb:69:in `run'
     # /Users/derrick/.rvm/gems/ruby-1.9.2-p0@player/gems/rspec-core-2.6.4/lib/rspec/core/runner.rb:11:in `block in autorun'
        should increment play count and track listen for the current user
      attempting to play a post with an error
        should mark a song as erroneous
      going back a song then going forward
        should play the next song that it once played before
  #previous
    in the main stream
      should return the previous song
      should increment play count and track listen for the current user

```
