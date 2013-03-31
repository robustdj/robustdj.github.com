---
layout: post
title: "Enable Logging to Rails Console"
date: 2010-10-09 23:02
comments: true
categories: 
  - ruby
  - rails
---

To have the rails log output to the console you can add this to your `~/.irbrc` file:

```ruby
require 'logger'

if ENV.include?('RAILS_ENV') && !Object.const_defined?('RAILS_DEFAULT_LOGGER')
  # for rails 2
  RAILS_DEFAULT_LOGGER = Logger.new(STDOUT)
elsif defined?(Rails)
  # for rails 3
  ActiveRecord::Base.logger = Logger.new(STDOUT)
end
```

This only enables the log on the machine you’re currently on. If you want to enable logging to the console as a project
wide feature so that anyone who checks out the project and runs the console also gets logging, then you can put this in
your `environment.rb` file (or an environment config file like `development.rb`):

```ruby
if "irb" == $0
  ActiveRecord::Base.logger = Logger.new(STDOUT)
end
```

