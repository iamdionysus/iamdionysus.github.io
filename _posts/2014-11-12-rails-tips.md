---
layout: post
title: Ruby on Rails Tips
---

## How to configure rails production server to display stylesheets correctly
If `rails server -e production` doesn't display stylesheets correctly, you need to change the configuration from `config/environments/production.rb`

```
config.server_static_assets = true
```

[See this answer](http://stackoverflow.com/questions/3939513/rails-production-server-stylesheets-not-displaying)


## How to connect multiple database
The best way to manage is by creating model as abastract class. If it's done like [this](http://stackoverflow.com/questions/15408285/rails-3-execute-custom-sql-query-without-a-model) in controller level, it might encounter connection pool problem. The other model should use your database.yml configuration but it's using the configuration in controller you visited.

In `databse.yml`

```yaml
analysis:
  adapter: sqlserver
  mode: dblib
  dataserver: something
  username: myname
  password: mypassword
  database: analysis
```

In `app/models/analysis.rb`

```ruby
class Analysis < ActiveRecord::Base
  self.abstract_class = true
  establish_connection "analysis"
```

Now you can query to the database like this inside any controller.

```ruby
Analysis.connection.select_all("SELECT * FROM serious_analysis")
```

Check these out.

* [2008 source](http://blog.bitmelt.com/2008/10/connecting-to-multiple-database-in-ruby.html)
* [official guide using SQL query](http://guides.rubyonrails.org/active_record_querying.html#finding-by-sql)

## How to find the first record with custom condition

```ruby
Client.find_by first_name: 'Lifo'

Client.where(first_name: 'Lifo').take
```

## How to use active record serializer in custom way
[ActiveModel::Serializer](https://github.com/rails-api/active_model_serializers) is efficient. You don't have to write many things but you can acheive many things. [Check this out](http://eewang.github.io/blog/2013/07/23/using-activemodel-serializers-to-build-great-json-interfaces/) how it's used.

## All about rails genearete model
### rails generate model field:type - available types
```
:primary_key, :string, :text, :integer, :float, :decimal, :datetime, :timestamp,
:time, :date, :binary, :boolean, :references
```

[Check this stackoverflow answer](http://stackoverflow.com/questions/4384284/rails-generate-model-fieldtype-what-are-the-options-for-fieldtype)

### How to set limit for field of integer to use bigint
```
rails generate model product quantity:integer{8}
```

[Check this blog](http://railsguides.net/advanced-rails-model-generators/)


## Message bus, to build real time web

* [how it's used in discourse](http://balinterdi.com/2014/01/14/how-real-time-updates-work-in-discourse.html)
* [github repo](https://github.com/SamSaffron/message_bus)
