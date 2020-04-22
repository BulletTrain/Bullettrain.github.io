---
title: Mac搭建Ruby环境
date: 2014-12-29 19:44:39
categories: iOS
tags: Ruby
copyright: true
---

## 安装系统需要的包

``` 
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## 安装RVM

``` 
$ curl -L https://get.rvm.io | bash -s stable

载入 RVM 环境
$ source ~/.rvm/scripts/rvm

检查一下是否安装正确
$ rvm -v
```

## 用RVM安装Ruby环境

``` 
目前最高最稳定版本是2.2.0
$ rvm install 2.2.0
```

## 设置Ruby版本

``` 
$ rvm 2.2.0 --default

测试是否正确
$ ruby -v

$ gem -v

$ gem source -r https://rubygems.org/
$ gem source -a https://ruby.taobao.org
```