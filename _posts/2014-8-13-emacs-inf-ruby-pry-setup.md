---
layout: post
title: How to make pry work with inf ruby
---

[set pry as default irb](http://blog.revathskumar.com/2012/11/set-pry-as-default-irb.html) by

* create ~/.irbrc if you don't have one
* add this

```rub
require 'pry'
Pry.start
exit
```
