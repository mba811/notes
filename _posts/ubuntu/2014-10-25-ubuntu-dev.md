---
layout: post
title: Ubuntu 下 Rbenv、Ruby、Rails、JAVA、Node 开发环境的搭建
tags: [ubuntu]
---

## Ubuntu系统下安装ruby/rails必要的库和编译环境

```
sudo apt-get update
sudo apt-get install -y build-essential openssl curl libcurl3-dev libreadline6 libreadline6-dev git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libxml2-dev libxslt-dev autoconf automake libtool imagemagick libmagickwand-dev libpcre3-dev libsqlite3-dev
```

## rbenv环境安装

```
git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc
type rbenv
git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
```

## ruby环境安装，首先列出可安装的版本，然后选择后进行下载编译

```
rbenv install -l
rbenv install 1.9.3-p448
```

## 设置当前使用的ruby版本并将gem的源改为淘宝镜像

```
rbenv global 1.9.3-p448
rbenv rehash
gem sources --remove https://rubygems.org/
gem sources -a http://ruby.taobao.org/
```

## 安装rails

```
gem install bundler rails
```

## 检查安装后的软件版本

```
ruby -v
gem -v
rake -V
rails -v
```

## 安装JAVA

```
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java6-installer
```

## 升级

```
#sudo update-java-alternatives -s java-6-oracle
```

## 设置JAVA环境变量

```
sudo apt-get install oracle-java6-set-default
```


## 设置JAVA字符

```
sudo mkdir /usr/lib/jvm/java-6-oracle/jre/lib/fonts/fallback
cd /usr/lib/jvm/java-6-oracle/jre/lib/fonts/fallback
sudo ln -s /usr/share/fonts/truetype/wqy/wqy-microhei.ttc wqy-microhei.ttf
sudo ln -s /usr/share/fonts/truetype/wqy/wqy-zenhei.ttc wqy-zenhei.ttf
```

## 安装node.js（已经包含npm）

```
sudo add-apt-repository ppa:chris-lea/node.js
sudo apt-get update
sudo apt-get install nodejs
```

##安装docker

**Ubuntu Trusty 14.04 (LTS) (64-bit)**

```
sudo apt-get update
sudo apt-get install docker.io

source /etc/bash_completion.d/docker.io

apt-get update
apt-get install apt-transport-https

sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9

sudo sh -c "echo deb https://get.docker.com/ubuntu docker main\
> /etc/apt/sources.list.d/docker.list"
sudo apt-get update
sudo apt-get install lxc-docker

或者

curl -sSL https://get.docker.com/ubuntu/ | sudo sh

sudo docker run -i -t ubuntu /bin/bash

ubuntu

exit

```