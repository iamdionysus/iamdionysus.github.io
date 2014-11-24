---
layout: post
title: How to use ember-cli with rails
---

## General Idea
This is based on [this tutorial.](http://reefpoints.dockyard.com/2014/05/07/building-an-ember-app-with-rails-part-1.html) I tried to follow the steps with details in my envrionment.

## Scaffolding project

### Create ember and rail project

```bash
mkdir my-app
cd my-app
ember new my-app-ember
rails new my-app-rails --database=sqlserver --git
```

### Edit Gemfile, database.yml, config/application.rb in rails project
Add `gem 'tiny_tds'` and remove `jquery-rails`, `turbolinks`, `coffee-rails`, `jbuilder`. After that `bundle update`, edit database.yml settings and test run it.

```bash
cd ~/my-app/my-app-rails
rails server
rails server -e production

cd ~/my-app/my-app-ember
ember server
```

In `config/application.rb` add these lines to prevent `rails generate controller` to generate javascript and css. [See this.](http://stackoverflow.com/questions/7366273/how-do-i-turn-off-automatic-stylesheet-javascript-generation-on-rails-3-1)

```
config.generators.stylesheets = false
config.generators.javascripts = false
```

## Creating backend rails test REST api
### Scaffold model, controller, serializer

```
rails generate model Tester name:string email:string
rails generate serializer Tester
rails generate controller Testers index
```

### Edit serializer, controller, routes
In `app/serializers/tester_serializers.rb` change `attributes :id` to `attributes :name, :email`. In `app/controllers/api/testers_controller.rb` add `render json: Tester.all` in the index method. In `config/routes.rb` make sure to have this.

```ruby
namespace :api do
  get 'testers', to: 'testers#index'
end
```

### Seed some data to seed
In `db/seeds.rb` add

```ruby
testers = Tester.create([{name: 'my name', email: 'test@test.com'}])
```

```bash
rake db:migrate
rake db:seed
```

### Let's test
```
rails server
```

Connect to `localhost/api/testers`
