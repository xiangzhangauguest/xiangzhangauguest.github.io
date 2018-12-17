---
title: "Hive External Table"
layout: post
category: [人生经验]
tags: [hive]
excerpt: "Hive 外部表的一些操作"
---

# Create

```sql
create EXTERNAL table applogsnew
(
applogid string,
msgtype string,
clienttype string,
userid bigint
)
PARTITIONED BY (create_time string) 
row format delimited
fields terminated by '\t'
stored as textfile
location '/data/sda/apache-hive-1.2.1-bin/tmp/warehouse/applogsnew';
```

# Upload data

```shell
hadoop fs -mkdir /data/sda/apache-hive-1.2.1-bin/tmp/warehouse/applogsnew/create_time=20160531

hadoop fs -put /home/appadmin/web/000000_0 /data/sda/apache-hive-1.2.1-bin/tmp/warehouse/applogsnew/create_time=20160531/

alter table applogsnew add partition (create_time='20160531') location '/data/sda/apache-hive-1.2.1-bin/tmp/warehouse/applogsnew/create_time=20160531';
```

如果有多个partition，则用,分隔即可。



