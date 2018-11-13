---
title: "Python regular expression"
layout: post
category: [人生经验]
tags: [python, regex]
excerpt: "记录python正则用法"
---

# re
```python
import re
str = "0 uuid_123:1 did_assd:1"

m = re.search("uuid_(.*?):1", str)
m.group(0) #uuid_123:1
m.group(1) #123
```
## 解释：
search 从任何位置搜索，只匹配一次

match 从开头开始匹配，可以指定开始匹配位置

findall 类似search，但是会匹配所有

?的作用： .*会greedy搜索，也就是会匹配到字符串最后，即使.*:这种形式，:也是没有用的。?会阻止greedy搜索。

()的作用：用于得到only regular expression part。
