---
layout: post
title: "Encapsulating Application Config Variables in a YAML File"
date: 2010-12-13 23:41
comments: true
categories:
  - rails
  - ruby
---

Having different configurations for different environments can get messy especially as your project grows. Environment files
start to get long and switching between all the different files becomes a hassle. One technique I like is to encapsulate all
of the configuration variables in a YAML file and then loading it up into a global hash during initialization.

I recently dealt with a case using paperclip and had different settings depending on the environment. On production servers
we wanted to use s3 for storage and locally we wanted to use the filesystem. At first we set up constants (to hold paperclip
settings) in each of our environment files. So we had something like this in each of our environment(`test.rb`,
`development.rb`, `production.rb`, etc) files:

```ruby
PAPERCLIP_STORAGE_OPTIONS = {
  :storage => :s3,
  :s3_credentials => "#{Rails.root}/config/s3.yml",
  :styles => { :original => "800x600>", :medium => "300x300#", :thumb => "100x100#"
  :path => "/user_images/:id/:style/:filename",
  :convert_options => { :all => "-strip" }
}
```

In translating it to YAML (`config/config.yml` in my case), it looked like this:

```yaml
paperclip_storage_options: &paperclip
  styles:
    original: "800x600>"
    medium: "300x300#"
    thumb: "100x100#"
  path: ":rails_root/public/user_images/:id/:style/:filename"
  convert_options:
    all: "-strip"

defaults: &defaults
  paperclip_storage_options:
    <<: *paperclip

deployed_defaults: &deployed_defaults
  <<: *defaults
  paperclip_storage_options:
    <<: *paperclip
    storage: "s3"
    s3_credentials: "<%= RAILS_ROOT %>/config/s3.yml"

development:
  <<: *defaults

test:
  <<: *defaults

qa:
  <<: *deployed_defaults

production:
  <<: *deployed_defaults
```

And so now each environment can have its own configurations and it’s nicely encapsulated in one file. You may notice that we
have embedded ruby in the YAML file. We just have to feed it into ERB when we load the YAML. Let’s take a look at
`config/initializers/config.rb`:

```ruby
APP_CONFIG = YAML.load(ERB.new(File.read("#{Rails.root}/config/config.yml")).result)[Rails.env].recursive_symbolize_keys
```
You can actually ignore the recursive_symbolize_keys, as I only had to do that since paperclip only takes symbolized
arguments. While this snippet of code isn’t the prettiest, it works well and enables global config encapsulation. First
it’s reading the YAML file, interpreting the ruby code, converting it into a hash, and then finally storing it into a
constant.

