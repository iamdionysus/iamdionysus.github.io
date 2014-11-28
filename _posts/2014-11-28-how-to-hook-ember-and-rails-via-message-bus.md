---
layout: post
title: How to hook ember front-end and rails back-end via MessageBus
---

## Introduction
[This post](http://balinterdi.com/2014/01/14/how-real-time-updates-work-in-discourse.html) shows how real-time updates work in discourse. Based on this explanation, I started to hook ember with rail via MessageBus. I'm using ember-cli.

## MessageBus in rails
### Install the gem
```
gem 'message_bus'
```

### Set up some test publish in rails controller
One of my test controller, I set up something like this to publish whenever I call GET on that route. If I visit `api/products/index` it'll publish some test message

```ruby
class Api::ProductsController < ApplicationController
  def index
    message_id = MessageBus.publish "/c", "my test message at #{rand}"
    render json: Product.all
  end
```

## MessageBus with ember-cli
### bower install the message-bus.js
ember-cli is managing dependencies [this way](http://www.ember-cli.com/managing-dependencies/). First, I can install the javascript like this [guide](http://bower.io/).

```
bower install https://raw.githubusercontent.com/SamSaffron/message_bus/master/assets/message-bus.js
```

### import the message-bus.js
edit `Brocfile.js` in the ember project root.

```
app.import('bower_components/message-bus/index.js');
```

### scaffold route, controller to use it
```
ember generate route message
ember generate controller message --type=object
```

### edit controller
The key is message-bus.js exports the MessageBus as window.MessageBus. So it will look like something like this. In `app/controllers/message.js`

```
import Ember from 'ember';

export default Ember.ObjectController.extend({
    message: "this",
    actions: {
        subscribe: function() {
            var bus = window.MessageBus;
            bus.start();
            var messageController = this;
            bus.subscribe("/c", function(data) {
                messageController.set('message', data);
            });
        }
    }
});

```

### edit template
In `app/templates/message.hbs`

```

<p>I got this message: {{message}}</p>


<button {{action 'subscribe'}}> Subscribe!! </button>

```
