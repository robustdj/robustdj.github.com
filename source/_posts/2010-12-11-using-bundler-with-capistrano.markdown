---
layout: post
title: "Using Bundler with Capistrano"
date: 2010-12-11 23:37
comments: true
categories: 
  - bundler
  - capistrano
  - rails
  - ruby
---

To make capistrano build your gems with bundler you can use the following code snippet:

```ruby
namespace :bundle do
  task :install, :roles => :app do
    shared_dir = File.join(shared_path, 'bundle')
    run "mkdir -p #{shared_dir}"
    run "cd #{release_path} && bundle install --without development test --path #{shared_dir}"
  end
end

after 'deploy:update_code', 'bundle:install'
```

Of course, this is assuming that you already have bundler installed on your remote server.

