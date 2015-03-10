---
layout: post
title: "Jekyll Setup"
modified:
categories: 
excerpt: “Quick shell script to set up jekyll on a new digital ocean box running ubuntu”
tags: [notes, digital_ocean]
image:
  feature:
date: 2015-03-09T22:41:54-07:00
---

# Quick shell script to set up jekyll on new ubuntu digital ocean droplet

## Code

{% highlight bash  %}

#!/bin/bash

# Setup Jekyll Blog on Digital Ocean Ubuntu 14.04 with Node Droplet

# https://www.digitalocean.com/community/tutorials/how-to-get-started-with-jekyll-on-an-ubuntu-vps

# Get key for ruby
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3

# Install ruby and rvm
curl -L https://get.rvm.io | bash -s stable --ruby=2.0.0

# Enable rvm in current shell
source /home/brian/.rvm/scripts/rvm

# Use right version of ruby
rvm install 1.9.3
rvm use 1.9.3 --default

# Install jekyll and capistrano
gem install jekyll --no-rdoc --no-ri 
gem install capistrano -v 2.15.5
gem install redcarpet

gem install -v 0.9.2 rubygems-bundler

# Install generator of jekyll
sudo npm install -g generator-jekyllrb

# Install grunt
sudo npm install -g grunt
sudo npm install -g grunt-cli

npm install grunt-contrib-concat 

# Install bower
sudo npm install -g bower

# Create a new directory and go to it

mkdir -p blog && cd blog

# Run the generator
# HTML5 template option seems broken so don’t use it
# Choose defaults except choose maruku for templating library
yo jekyllrb

# Install packages
bower install && npm install

# ========================== Setting up nginx server ============

sudo apt-get -y install nginx
sudo service nginx start
ifconfig eth0 | grep inet | awk ‘{ print $2 }’
update-rc.d nginx defaults

sudo ln -s /etc/nginx/sites-available/brian-blog.io /etc/nginx/sites-enabled/brian-blog.io

sudo chown -R www-data:www-data /home/brian/blog/
sudo chmod 755 -R /home

{% endhighlight %}
