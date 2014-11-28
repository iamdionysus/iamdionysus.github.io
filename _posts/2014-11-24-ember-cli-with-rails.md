---
layout: post
title: How to use ember-cli with rails
---

Based on [this tutorial](http://reefpoints.dockyard.com/2014/05/07/building-an-ember-app-with-rails-part-1.html), I tried to follow the steps with details in my envrionment.

## Scaffolding project

### Create ember and rails project

```bash
mkdir my-app
cd my-app
ember new my-app-ember
rails new my-app-rails --database=sqlserver --git
```

### Edit Gemfile, database.yml, config/application.rb in rails project

* Add `gem 'tiny_tds'` and remove `jquery-rails`, `turbolinks`, `coffee-rails`, `jbuilder`from Gemfile
* `bundle update`
* Edit database.yml settings and test run it.

```bash
cd ~/my-app/my-app-rails
rails server
rails server -e production

cd ~/my-app/my-app-ember
ember server
```

In `config/application.rb` add these lines to prevent `rails generate controller` to generate javascript, css assets and view.

* [doing this in command line](http://stackoverflow.com/questions/18406370/dont-create-view-folder-on-rails-generate-controller)
* [old example for config/application.rb](http://stackoverflow.com/questions/7366273/how-do-i-turn-off-automatic-stylesheet-javascript-generation-on-rails-3-1)
* [documentation](http://guides.rubyonrails.org/configuring.html)

```ruby
config.generators do |g|
  g.assets false
  g.helper false
  g.template_engine false
end
```

## Creating backend rails sinmple get REST api for test

### Scaffold model, controller, serializer

```
rails generate model Tester name:string email:string
rails generate serializer Tester
rails generate controller Testers index
```

### Edit serializer, controller, routes
#### serializer
In `app/serializers/tester_serializers.rb`, change `attributes :id` to `attributes :name, :email`.

#### controller
In `app/controllers/api/testers_controller.rb`, add `render json: Tester.all` in the all method. 

```ruby
class Api::TestersController < ApplicationController
  def all
    render json: Tester.all
  end
end
```

#### routes
In `config/routes.rb` make sure to have this.

```ruby
namespace :api do
  get 'testers', to: 'testers#index'
end
```

#### Seed some data to seed
In `db/seeds.rb` add

```ruby
testers = Tester.create([{name: 'my name', email: 'test@test.com'}])
```

```bash
rake db:migrate
rake db:seed
```

#### run server and see how it goes
run `rails server` and connect `localhost/api/testers`


## How to use multiple databases by scaffolding model as abstract_class
### set up database.yml

```yaml
analysis:
  <<: *default
  username: name
  password: pw
  database: Analysis
```

### scaffold model without fixture, migration and set it up
#### scaffold model without fixture, migration
```bash
rails generate model Analysis --no-fixture --no-migration
```

#### edit the app/models/analysis.rb

```ruby
class Analysis < ActiveRecord::Base
  self.abstract_class = true
  establish_connection :analysis
end
```

#### write simle test and run
edit `test/models/analysis_test.rb` and run `rake test`

```ruby
require 'test_helper'

class AnalysisTest < ActiveSupport::TestCase
  test "SELECT TOP 1 * FROM populations"
    query = "SELECT TOP 1 * FROM populations"
    result = Analysis.connection.select_all query
	assert_equal result.count, 1
  end
end
```

### scaffold controller and set it up
#### scaffold controller
```bash
rails generate controller API::Populations top_500s
```

#### make sure it has the right routes
open `config/routes.rb` and check it out

```ruby
namespace :api do
  get 'testers', to: 'testers#index'
  get 'populations/top500s'
```

#### edit controller
open `app/controllers/api/populations_controller.rb` and edit top_500s method

```ruby
def top_500s
  query = "SELECT TOP 500 * FROM populations"
  result = Analysis.connection.select_all query
  render json: {top_500s: result}
end
```

