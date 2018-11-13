---
title: "Linux Shell 笔记"
layout: post
category: [人生经验]
tags: [linux, shell]
excerpt: "这里做一些Linux Shell的笔记，备忘。"
---

### ${}变量替换
一般情况下，```$var```与```${var}```是没有区别的，但是用${ }会比较精确的界定变量名称的范围。
```sh
$ A=B
$ echo ${A}B
BB
```

### command substitution
将命令输出的结果写入变量:
```sh
output=$(ls -l)
echo $output
```


### 函数定义
```sh
func_name () {
    command
}
```

### split string
```sh
A="$(cut -d'_' -f2 <<<'one_two_three_four_five')"
```
或者
```sh
A="$(echo 'one_two_three_four_five' | cut -d'_' -f2)"
```


### read line by line from file
```sh
while IFS='' read -r line; do
    echo "Text read from file: $line"
done < "$file"
```
or
```sh
while IFS='' read -r line || [[ -n "$line" ]]; do
    echo "Text read from file: $line"
done < "$1"
```

Explanation:

```IFS=''``` (or ```IFS=```) prevents leading/trailing whitespace from being trimmed.

```-r``` prevents backslash escapes from being interpreted.

```|| [[ -n $line ]]``` prevents the last line from being ignored if it doesn't end with a \n (since  read returns a non-zero exit code when it encounters EOF).



