---
layout: post
title: "Sunspot (Solr) on Rails 3"
date: 2010-12-06 23:30
comments: true
categories: 
  - rails
  - ruby
  - solr
---

Sunspot is a ruby driver for [Solr](http://lucene.apache.org/solr/) and is pretty easy to integrate into rails using
[sunspot_rails](https://github.com/outoftime/sunspot_rails).

##Installation

First add it to your Gemfile:

```ruby
gem "sunspot_rails", "1.2.rc4"
```

Update your bundle:

```ruby
bundle install
```

Run this task to create your `sunspot.yml` file:

```
rails g sunspot_rails:install
```

To start the sunspot server run:

```
rake sunspot:solr:start RAILS_ENV=development
```

You probably don’t need the rails env variable set but I usually do it to be explicit. The first time you run this command,
it will create a `solr/ directory` in your Rails root. This contains Solr’s configuration, as well as the actual data files
for the Solr index. You’ll probably want to add `solr/data` to your `.gitignore`.

##Setting up your Models

There are two ways to setup your models for indexing. First way (and more appropriate for rails) is to use the ‘searchable’ macro. Suppose we have a model called Provider with first_name, middle_name, and last_name columns in the database. We can setup those fields to indexing with the following:

```ruby
class Provider < ActiveRecord::Base
  searchable do
    text :first_name :middle_name, :last_name
  end
end
```

The second way is the following:

```ruby
Sunspot.setup Provider do
  text :first_name, :middle_name, :last_name
end
```

This code can be stored at the end of the model (outside of the class), or in an initializer, though it isn’t too pretty.
For more information on setting up classes with search and indexing, check out this [page](https://github.com/outoftime/sunspot)

##Don’t Forget to Reindex

To reindex, run the following rake task:

```ruby
rake sunspot:reindex RAILS_ENV=development
```

You can also reindex in the console by running:

```ruby
Provider.reindex
```

##Testing Provider search with RSpec

Normally I use `before(:each)` blocks to set up my data, but setting up search data is a use case for using a `before(:all)`
block. With search, we’re not manipulating the data, so having test data setup before all search specs is not only
acceptable, but efficient. Below are specs for testing that a provider can be found by all parts of his/her name:

```ruby
describe "search" do
  before(:all) do
    @provider = Factory(:provider, :first_name => "Dexter", :middle_name => "Berry", :last_name => "Pilsner")
    Provider.reindex
  end

  it "should find a provider by first name" do
    Provider.search{ fulltext 'dexter' }.results.should == [@provider]
  end

  it "should find a provider by middle name" do
    Provider.search{ fulltext 'berry' }.results.should == [@provider]
  end

  it "should find a provider by first name" do
    Provider.search{ fulltext 'pilsner' }.results.should == [@provider]
  end
end
```

One gotcha. Don’t forget to start your sunspot server in the test environment:

```
rake sunspot:solr:start RAILS_ENV=test
```

