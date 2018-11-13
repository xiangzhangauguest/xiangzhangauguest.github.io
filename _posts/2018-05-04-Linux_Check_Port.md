---
title: "Linux Check Port"
layout: post
category: 人生经验
tags: [linux]
excerpt: "Linux how to check port is used."
---

# lsof
lsof (list open files), 用于查看你进程开打的文件，打开文件的进程，进程打开的端口(TCP、UDP)。
找回/恢复删除的文件。是十分方便的系统监视工具，因为 lsof 需要访问核心内存和各种文件，所以需要root用户执行。

```sh
lsof -i:123 # 查看123端口是否被使用
```
