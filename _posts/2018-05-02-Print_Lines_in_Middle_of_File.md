---
title: "Print lines in the middle of file"
layout: post
category: [人生经验]
tags: [linux, sed]
excerpt: "打印文件的中间行"
---

```sh
sed -n '1000,2000;2001q' file #打印file的1000-2000行
```
