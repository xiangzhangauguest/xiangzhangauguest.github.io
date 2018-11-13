---
title: "Hive Command Not Found in Crontab"
layout: post
category: 疑难杂症
tags: [linux, hive, crontab]
excerpt: "Crontab中找不到Hive命令解决。"
---


写了一个shell 脚本，里面用了hive hadoop的一些操作，然后手动执行shell是没问题的，但是起cron执行的话，会报下面的错误:

hive: command not find

hadoop: command not find



## 问题原因

crontab 的环境变量和自己运行时的环境变量不同导致

## 解决方法

网上找到的方法：

1. shell脚本里加 source /etc/profile。
2. 然后将hive  hadoop命令改成绝对路径。

新浪机器上试过之后发现还是执行cron出错，找了几个机器上其他脚本，他们使用的是下面的做法：

1. shell脚本里加 1）. /etc/bashrc 2）. ~/.bash_profile 3） source /etc/profile

这种方法成功了。

Linux bashrc bash_profile等文件的说明在另一个博客里(https://zhangxiangjor.github.io/%E4%BA%BA%E7%94%9F%E7%BB%8F%E9%AA%8C/Linux_Bashrc_File_Explaination)。
