---
title: "Hive Result into File"
layout: post
category: [人生经验]
tag: [hive, linux]
excerpt: "把hive的结果写入文件，并且用逗号分隔字段"
---

```sh
hive -e 'set hive.cli.print.header=true; select * from your_Table' | sed 's/[\t]/,/g'  > /home/yourfile.csv
```

set hive.cli.print.header=true

导出字段名称

sed 's/[\t]/,/g'

hive 结果默认用tab分隔字段，sed将tab替换为逗号
