---
title: "Linux add directory into PATH"
layout: post
category: [人生经验]
tags: [linux]
excerpt: "Linux加入PATH的方法。"
---

安装好软件后，有时需要将bin目录加入PATH，大概有三种方法。

修改内容:
PATH=$PATH:XXX

### 对某个user添加
需要修改这个用户home目录下的 .bash_profile文件。

### 对所有用户添加
需要修改/etc/profile文件。

### 对root用户添加
需要修改root用户的home目录下.bash_profile文件。

如果修改之后当前terminal未生效，需要source ~/.bash_profile

参考：http://www.troubleshooters.com/linux/prepostpath.htm
