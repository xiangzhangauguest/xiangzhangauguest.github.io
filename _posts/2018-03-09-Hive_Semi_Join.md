---
title: "Hive Semi Join"
layout: post
category: [人生经验]
tag: [hive]
excerpt: "hive semi join用法"
---

```sql
SELECT a.key, a.value
FROM a
WHERE a.key in
 (SELECT b.key
  FROM B);
```

等价于：

```sql
SELECT a.key, a.val
FROM a LEFT SEMI JOIN b ON (a.key = b.key)
```
