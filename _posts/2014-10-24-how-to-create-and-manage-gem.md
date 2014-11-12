---
layout: post
title: How to create and manage ruby gem
---

## Use bundler to scaffold

```
bundle gem my_gem
```

To understand what it deos, take a look at FOLDER STRUCTURE and GEMSPEC section in [this tutorial](http://www.smashingmagazine.com/2014/04/08/how-to-build-a-ruby-gem-with-bundler-test-driven-development-travis-ci-and-coveralls-oh-my/) Also, [this guide](http://guides.rubygems.org/make-your-own-gem/) is worthwhile to read. For the full specification of Gem in the .gemspec file or a Rakefile, [check this out.](http://guides.rubygems.org/specification-reference/)

## User bundler to build the gemfile

```
gem build my_gem.gemspec
```

## Install gem from github source
In the Gemfile for the project, add the following.

```
gem 'redcarpet', :git => 'git://github.com/tanoku/redcarpet.git'
gem 'redcarpet', :git => 'git://github.com/tanoku/redcarpet.git', :branch => 'branch-to-install'
gem 'redcarpet', :git => 'git://github.com/tanoku/redcarpet.git', :tag => 'tag_to_install'
gem 'gem_in_gitlab', :git => "http://username:#{ENV['g_pass']}@gitlab.internal/pass/to/repo/gem_in_gitlab.git"
```

[It should have .gemspec in there.](http://stackoverflow.com/questions/2577346/how-to-install-gem-from-github-source)
