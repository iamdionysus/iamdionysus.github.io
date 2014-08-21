---
layout: post
title: Tips from everyday encounter
---

## Google analytics says "Tracking Not Installed"
I copy and pasted the tracking code to jekyll _includes/head.html. However, I wasn't sure about whether it's really installed or not. I followed the google's instruction but what I see was "Tracking Not Installed". **It will be showing correct status after a while** according to the [stackoverflow answer](http://stackoverflow.com/questions/16952379/google-analytics-says-tracking-not-installed-but-i-see-it-working). Quick way to check is [visiting the Real-Time section](https://support.google.com/analytics/answer/1638635?hl=en) in the analytics menu.

## Auto install Emacs packages
Whenever I reinstall Emacs, I had to list-packages and check the packages that I have to install to maintain the system as identical as possible. Recently I found [use-package](https://github.com/jwiegley/use-package) and **auto install Emacs package can be done easily by using the use-package** [according to stackoverflow answer.](http://stackoverflow.com/questions/21064916/auto-install-emacs-packages-with-melpa/21342883#21342883) Actual init.el file sample which is using this can be found [here](https://github.com/jordonbiondo/.emacs.d/blob/master/init.el).


## How to configure Emacs conditional to operating system
Multi-term is only working for linux. I want to install multi-term only when I use Emacs under linux and bind keys as well. The key is [checking system-type in elisp.](http://stackoverflow.com/questions/1817257/how-to-determine-operating-system-in-elisp)

```
(if (eq system-type 'gnu/linux)
  (use-package multi-term
    :bind (("C-M-t" . multi-term))
    :ensure t)
)
```

## Getting rid of "DL is deprecated, please use Fiddle" message
When running IRB or Pry under windows, this warning message shows up. If it's too annoying, it can be fixed by [editing lib/ruby/site_ruby/2.0.0/readline.rb.](http://stackoverflow.com/questions/15590450/ruby-2-0-0p0-irb-error-dl-is-deprecated-please-use-fiddle) Alternatively, you can install [rb-readline gem and add configuration to .pryrc and irb.](http://zcoding.blogspot.sg/2014/04/get-rid-of-dl-deprecated-message-from.html)

## Add all untracked files to the staging area under magit-status
[See the manual.](http://magit.github.io/magit.html#Untracked-files)

* Cursor should be right at Untracked files section title and hit ```s```
* Cursor can be any place but hit ```C-u S```(large capital)
