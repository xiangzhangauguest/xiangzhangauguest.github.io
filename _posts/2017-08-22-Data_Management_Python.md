---
title: "Python的数据处理"
layout: post
category: [人生经验]
tags: [python, data]
excerpt: "记录一些python处理数据的方法，备忘。"
---

# pandas

### concat two data frames
```py
df_all = pd.concat([df1, df2])
```
### select columns
```py
df_some_columns = df[["col1", "col2"]]
```
### merge two data frames
```py
df_merged = pd.merge(df1, df2, on="id", how="left")
```
### copy a data frame
```py
df_copy = df.copy()
```
### unique
```py
df["id"].unique() # get unique id.
df["id"].nunique() # get number of unique id.
```
### NaN
```py
df.isnull() # get a data frame which elements are true/false.
df.isnull().sum() # get a series which indexes are column names, values are number of null elements in this column.
df.isnull().sum().sum() # get a integer which is the number of null elements in data frame.
```
