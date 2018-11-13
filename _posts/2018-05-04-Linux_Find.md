---
title: "Linux Find"
layout: post
category: [人生经验]
tags: [linux]
excerpt: "Linux find 用法"
---

```sh
find -name "query" # 查找query名的文件
find -iname "query" ＃ ignore the case of query
```

```sh
find -type type_descriptor query
find / -type c # find all c type from /
find / -type d -name dir # find directory named "dir" from / 
find / -type d -name dir 2 > /dev/null # eliminate all the error messages you'll likely (read, always) get when not doing this as the root user
```
type_descriptor:

f: regular file

d: directory

l: symbolic link

c: character devices

b: block devices

[reference](https://www.digitalocean.com/community/tutorials/how-to-use-find-and-locate-to-search-for-files-on-a-linux-vps)
