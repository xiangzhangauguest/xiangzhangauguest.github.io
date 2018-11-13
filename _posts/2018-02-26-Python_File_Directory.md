---
title: "Python File Directory"
layout: post
category: [人生经验]
tag: [python]
excerpt: "python获取文件的所在目录路径"
---

/test/sub/sub_path.py

获取sub_path.py所在目录正确方法：
```py
os.path.split(os.path.realpath(__FILE__))[0]
```

[参考](http://blog.csdn.net/linzch3/article/details/71250421)
