---
title: "Linux uniq"
layout: post
category: [人生经验]
tags: [linux]
excerpt: "记录一些linux uniq的用法，备忘。"
---

```sh
sort File | uniq  #去掉重复的
sort File | uniq -c #去掉重复的，但是每行打印出现的次数
sort File | uniq -cd #只打印重复的
sort File | uniq -c | sort -nr #按出现的次数从高到底排序
```
