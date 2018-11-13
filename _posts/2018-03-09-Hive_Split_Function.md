---
title: "Hive Split Function"
layout: post
category: [人生经验]
tag: [hive]
excerpt: "hive 的split函数介绍"
---

```sql
hive -e "... split(127.0.0.1, "\\\\.") ..."
```


split(string str, string pat)

   Split str around pat (pat is a regular expression) 
   
hive 的split函数可以分割字段，需要注意的是split支持正则，所以如文章开始的例子所示，如果要匹配```'.'```，需要用```'\\.'```，在```""```
里面的话就需要再在每个```'\'```前加一个```'\'```，因此变成了```'\\\\.'```
