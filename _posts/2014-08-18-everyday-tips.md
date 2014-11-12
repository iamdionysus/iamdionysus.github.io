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
[Guide from official website](http://git-scm.com/book/en/Getting-Started-Installing-Git) and applied to CentOS case

```
yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
cd ~
wget -O git.zip https://github.com/git/git/archive/master.zip
unzip git.zip
cd git-master
make prefix=/usr/local all
sudo make prefix=/usr/local install
```

To update git, make the source repository as git.

```
git clone git://github.com/git/git
```

## sudo: gem: command not found error in CentOS
After I install ruby from the source as root user in CentOS, I tried to install gem from other sudoer user by `sudo gem install tiny_tds`. However, i get the error. It's because gem lives in the /usr/local/bin from the default ruby installation setting. Simple solution will be adding symbolic link in the /sbin or /usr/sbin. `sudo ln -s /usr/local/bin/gem /sbin/gem` worked out. [This](http://tutcenter.com/linux/centos/fixing-sudo-service-command-not-found/) was not the case but gave me a clue to tackle this error.

## How to sync a fork to upstream repository
[Configure upstream](https://help.github.com/articles/configuring-a-remote-for-a-fork/) and [merge](https://help.github.com/articles/syncing-a-fork/). If the upstream should be always right, merge it by `git merge --strategy-option theirs`. I would recommend to do this way first and compare forked master to my forked branch and handle the merge with the web interface.

## How to install freetds and test it
### CentOS 7
#### Install the EPEL repository
```
wget http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-2.noarch.rpm
sudo rpm -Uvh epel-release-7*.rpm
```

#### Install freetds
This is not tested fully since I am not sure [Remi Repo is needed.](http://bloke.org/linux/freeds/) The following looks like working in CentOS 7.
```
sudo yum install freetds freetds-devel
```

#### Edit freetds.conf file
The default freetds.conf location is `/usr/local/etc/freetds/conf` Open the file and edit it. For sql server 2012 case, tds version = 8.0 is needed

```
[servername]
        host = 192.168.174.119
        port = 1433
        tds version = 8.0
```

If the tds version is not sure, we can test connection with this first and use it.

```
TDSVER=8.0 tsql -H ip_address -p 1433 -U username -P password
```

#### Check connection

```
tsql -S servername -U username -P password
```

[See this](http://www.freetds.org/userguide/confirminstall.htm)


## Manage multiple SSH keys
If I'm working  github, and internal gitlab site, it's tricky to make both happy without [SSH keys management.](http://www.robotgoblin.co.uk/blog/2012/07/24/managing-multiple-ssh-keys/)

### Create folder for each host

```
cd ~/.ssh
mkdir github
mkdir gitlab
```

### Generate SSH keys for each host

```
ssh-keygen -t rsa -C "my_email_for_github@example.com"
Generating public/private rsa key pair.
# Enter file in which to save the key (/home/you/.ssh/id_rsa): [Press enter]
```

Type `/home/you/.ssh/github/id_rsa` for github and do it again for the gitlab as well.

### Create config file and edit it

```
cd ~/.ssh
touch config
```

And it should look like this. Make sure Host matches with the actual host name of the server if it's running under windows mingw32. from `ssh -v git@internal.gitlab`, I saw the ssh was not be able to find IndentityFile if the Host is set up different name.

```
Host github.com
	User git
	Hostname github.com
	PreferredAuthentications publickey
	IdentityFile ~/.ssh/github/id_rsa

Host gitlab.com
# Not sure User git is needed, without it, it's working now
#	User git 
	Hostname internal.gitlab
	PreferredAuthentications publickey
	IdentityFile ~/.ssh/gitlab/id_rsa
```

### Register the id_rsa.pub to each host
```
cat ~/.ssh/github/id_rsa.pub
cat ~/.ssh/gitlab/id_rsa.pub
```
Paste the result.

### Test whether it's working
```
ssh -T git@github.com
```

### Trouble shooting
#### Bad owner or permissions on ~/.ssh/config
[See this answer.](http://serverfault.com/questions/253313/ssh-hostname-returns-bad-owner-or-permissions-on-ssh-config)

```
chmod 600 ~/.ssh/config
```

## How install emacs from source
[See this](http://linuxg.net/how-to-install-emacs-24-4-on-ubuntu-14-10-ubuntu-14-04-and-derivative-systems/)
