---
layout: post
title: How to make pry work with inf ruby
---

[set pry as default irb](http://blog.revathskumar.com/2012/11/set-pry-as-default-irb.html) by

* edit ~/.irbrc by adding the code below
  * create ~/.irbrc if you don't have one
  * on windows, it should be
    * Envrionment Variable HOME
	* or, %USERPROFILE% directory

```ruby
require 'pry'
Pry.start
exit
```
