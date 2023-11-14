---
layout: post
title: 配置本地BLOG
category: android
description: 
keywords: blog, Mac
---


##配置本地BLOG##
###Step 1.clone到本地
```
git clone git://github.com/lbwstco/lbwstco.github.com.git
```
###Step 2.将本地库与远程的资源库挂钩
```
git remote set-url origin git@github.com:lbwstco/lbwstco.github.com.git
```	
###Step 3.配置账户信息
```
git config --global user.name "你的名字"  
git config --global user.email "youremail@youremail.com" 
```
###Step 4.配置SSH
```
ssh-keygen -t rsa -C "youremail@youremail.com"
```


拷贝~/.ssh/id_rsa.pub里的内容至Github设置的SSH Key

