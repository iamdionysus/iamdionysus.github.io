---
layout: post
title: Tips from everyday encounter
---

## Google analytics says "Tracking Not Installed"
I copy and pasted the tracking code to jekyll _includes/head.html. However, I wasn't sure about whether it's really installed or not. I followed the google's instruction but what I see was "Tracking Not Installed". **It will be showing correct status after a while** according to the [stackoverflow answer](http://stackoverflow.com/questions/16952379/google-analytics-says-tracking-not-installed-but-i-see-it-working). Quick way to check is [visiting the Real-Time section](https://support.google.com/analytics/answer/1638635?hl=en) in the analytics menu.


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

## How to move through the ido list in emacs
C-s, C-r


## Ruby heredoc, how to create pretty multi line string
```ruby
execute <<-SQL
  ALTER TABLE students
	ADD CONSTRAINT pk_students_roll_no_name
	PRIMARY KEY (roll_no, name)
SQL
```

## SQL PRIMARY KEY Constraint, CREATE and DROP
When drop a primary key constraint, mysql has different syntax. [Check this out.](http://www.w3schools.com/sql/sql_primarykey.asp)


## How to add user in CentOS

```
useradd <username>
passwd <username>
```


## How to add sudo to user
Run `sudo visudo` and edit the file in ubuntu. `sudo /usr/sbin/visudo` in CentOS.

```
# User privilege specification
root      ALL=(ALL:ALL) ALL
username  ALL=(ALL:ALL) ALL
```

## rails spring conflict from Permission denied @ rb_sysopen
[stackoverflow answer](http://stackoverflow.com/questions/23822491/ruby-on-rails-permission-denied-when-using-rails-generate-controller-welcome)

```
sudo chmod -R 1777 /tmp
```

## How to install latest git
[This is CentOS guide.](https://www.digitalocean.com/community/tutorials/how-to-install-git-on-a-centos-6-4-vps)

```
sudo yum groupinstall "Development Tools"
sudo yum install zlib-devel perl-ExtUtils-MakeMaker asciidoc xmlto openssl-devel
cd ~
wget -O git.zip https://github.com/git/git/archive/master.zip
unzip git.zip
cd git-master
make configure
./configure --prefix=/usr/local
make all doc
sudo make install install-doc install-html
```

To update git, make the source repository as git. `git clone git://github.com/git/git`

