---
layout: post
title: New Ruby On Rails Tutorial with sqlserver under windows and linux environment
---

## Why?
Ruby on rails tutorial from Michael Hartl is good. It is well structured with concrete example. However, **there are many outdated instructions** since his recent one is based on rails 4.0. For example, Rails 4.1 comes with Spring for application preloader. Trying to follow the Spork instruction in his book is a bit tough under the Rails 4.1 environment. And Rspec as well. **I will follow each steps from his tutorial but with simpler and newer instructions**.

## Install ruby and set up Rails
### Ubuntu 14.04
After I play around with rvm and rbenv, I settled down to rbenv. [Good instructions to install and setup on Ubuntu 14.04.](https://gorails.com/setup/ubuntu/14.04)

### Windows
I installed it with [RubyInstaller.](http://rubyinstaller.org/downloads) To install Jekyll on Windows I had to install Ruby DevKit as well. It might be needed in the future for gems requiring the dependency. [Here is the instruction to install Ruby Devkit from Jekyll on windows](http://jekyll-windows.juthilo.com/1-ruby-and-devkit) 

### Gems
Installing rails gem is simple. ```gem install rails``` To use sqlserver from Rails, we need more gems. [Follow the instruction from TinyTDS.](https://github.com/rails-sqlserver/activerecord-sqlserver-adapter/wiki/Using-TinyTds)

## Test the Rails setup
### Scaffold the project and test run it

```ruby
rails new my-project-name --database=sqlserver --skip-test-unit
rails server
``` 

On windows 64-bit ruby, ```No source of timezone data could be found. (TZInfo::DataSourceNotFound)``` error will pop up.  [This can be fixed by configuring Gemfile.](http://stackoverflow.com/questions/23022258/tzinfodatasourcenotfound-error-starting-rails-v4-1-0-server-on-windows) After editing the Gemfile, don't forget to do ```bundle update```.


### Change the database configuration in database.yml and test the connection

```yaml
default: &default
	# I'm not using the dataserver. So, I have to add host
	# dataserver: from_freetds.conf 
	host: your_ip_or_host_name
	username: your_name
	password: your_password
```

To test the database connection is working, create test model and run rails console to play around with the model.

```ruby
rails generate model User name:string email:string
rake db:migrate
rails console --sandbox

>> user = User.new(name: 'Test', email:'test@test.com')
=> #>User id:nil, name: "test", ...
>> user.save
SQL (40.0ms) SAVE TRANSACTION ...
=> true
>> User.all
User Load (41.0ms) ...
=> #<ActiveRecord::Relation ...
>> exit
```

Done. rollback the database from the test and destroy the test model as well.

```
rake db:rollback
rails destroy model User
```

