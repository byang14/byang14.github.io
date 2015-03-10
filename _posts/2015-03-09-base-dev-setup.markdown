---
layout: post
title: "Base Development Environment Setup"
modified: 
categories: 
excerpt: “Quick shell script to set up base development environment on new digital ocean droplet covering zsh, spf13, and docker”
tags: [notes, digital_ocean, ubuntu]
image:
  feature:
date: 2015-03-09T22:42:54-07:00
---

####Quick shell script to set up base development environment on new digital ocean droplet

Covers zsh, spf13, and docker

## Code

{% highlight bash  %}

#!/bin/bash
# Setup Base Dev Environment in Ubuntu 14.04 Digital Ocean

# ================= Update Current Software ==============

sudo apt-get -y update
sudo apt-get -y upgrade

# ================= Add New User =========================

# Add new user
adduser brian

# Switch user password

# Only works on Debian
# echo -e ‘password_here\npassword_here’ | (passwd --stdin brian)

# Works Ubuntu
echo brian:password_here | chpasswd

# Switch to new user
su brian

# ================= Keyless SSH ==========================

# Create .ssh directory
mkdir -p ~/.ssh

# Add public key to directory
cat >> ~/.ssh/authorized_keys <<EOF
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC1sjmopE0kD3nuVbYkSi3Jn3iFUf+SWjxSmyyE5J368afE0WdXO9CZNHcM5+SjbhKHGDXeaosTzuijGM/X6bgWmhBzOIT7kqlFqtSgnAWxoyYrJek7cEuO/qDl8v+XgBTJ/l5hhvAv43LzWbo6wRN7a8mfY0F63GS68qljpyc90OtUt4oaXMe2c2/10BCLFPyb2CaffY4Ruf4+jC/25Bu82zKMv6KeRFjHmQMQZV5ShsWKBz84CSkxYbla1esgQhjGwHq0lzZdoitGCpPSSnISzHJ+mxdFcvo6lt4AJqG9pq1tli+6YYIiU9JCM1ZCp2lXsqQ2QG3LsZYd4i6MHXXn brianyang@Administrators-MacBook-Pro.local
EOF
cp ~/.ssh/authorized_keys ~/.ssh/authorized_keys2

# ================== Setup Oh My Zsh ======================

# Install zsh and git
sudo apt-get -y install zsh
sudo apt-get -y install git-core

# Get latest installation and run it
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh

# Change shells
chsh -s `which zsh`

#==================== Install spf13 fork ===================

# essentials
sudo apt-get install -y build-essential cmake python-dev silversearcher-ag

# vim
git clone https://github.com/byang14/spf13-vim.git
sudo sh ~/spf13-vim/bootstrap.sh
sudo cp ~/spf13-vim/.vimrc.local ~/ && cp ~/spf13-vim/.vimrc.bundles.local ~/

#==================== Configure tmux =======================

# tmux config
cp ~/spf13-vim/tmux/.tmux.conf ~/
tmux source-file ~/.tmux.conf

## install fonts for vim-airline (then choose in terminal preference)
sudo sh ~/spf13-vim/fonts/install.sh>>

{% endhighlight %}
