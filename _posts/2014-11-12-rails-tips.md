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
