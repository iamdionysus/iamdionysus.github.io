---
layout: post
title: How to make pry work with inf ruby
---

[set pry as default irb](http://blog.revathskumar.com/2012/11/set-pry-as-default-irb.html) by

* create ~/.irbrc if you don't have one
* on windows, ~/ folder should be
  * Envrionment Variable HOME
  * or, %USERPROFILE% directory
* edit ~/.irbrc by adding the code below

```ruby
require 'pry'
Pry.start
exit
```
