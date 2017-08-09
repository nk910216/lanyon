---
layout: post
title: 在Ubuntu 14.04 上裝 Jekyll
---

> 之前荒廢了一陣子，最近又想回來寫寫文章。這次想在自己的 Ubuntu 上也安裝 Jekyll, 這篇紀錄一下整個流程。

**整篇的重點就是 [這篇教學](https://hanbingyan.github.io/2016/04/04/jekyll/)，網路真是個方便的東西。**  
以下的東西跟他網站的教學一模一樣，只是以防一我自己留一個紀錄。

<!-- more -->  

## **Install rbenv**

~~~ bash
$ git clone git://github.com/sstephenson/rbenv.git ~/.rbenv
$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
$ echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc
~~~

## **Install ruby-build** 

~~~ bash
$ git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
$ echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
$ source ~/.bashrc
~~~

## **Install rbenv-gem-rehash**

~~~ bash
$ clone https://github.com/sstephenson/rbenv-gem-rehash.git ~/.rbenv/plugins/rbenv-gem-rehash
~~~

## **Install Node.js**

這邊的 **-y** 我需要拿掉才不會出現 error

~~~ bash
$ curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
$ sudo apt-get install -y nodejs
~~~

## **Install Ruby and Jekyll**

check and update first  

~~~ bash
$ sudo apt-get update
$ sudo apt-get install build-essential libssl-dev libreadline-dev libyaml-dev libxml2-dev libxslt1-dev libffi-dev python-software-properties
~~~

Install ruby and Jekyll  

~~~ bash
$ rbenv install 2.2.3
$ rbenv global 2.2.3
$ gem install bundle
$ gem install jekyll
~~~

## **Finish, HAVE FUN!!!**