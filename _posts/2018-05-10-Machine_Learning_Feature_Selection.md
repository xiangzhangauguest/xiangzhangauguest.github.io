---
title: "Machine Learning Feature Selection"
layout: post
category: 人生经验
tags: [ML]
excerpt: "通用的特征选择方法"

---
# 综述


## 此种类型的reference会认为自己引用的是const的变量，因此不能通过reference改变被引用的变量的值。
```cpp
const int a = 2;
const int &b = a; // right.
b = 6; // wrong.
```

```cpp
int a = 2;
const int &b = a; // right.
```

## 人们说的const reference实际上是reference to const
由于reference不是object，所以它没有const，人们为了顺口说的const reference其实是reference to const。

## 禁止reference的类型与被引用变量的类型不一致
```cpp
double a = 2;
const int &b = *a; // wrong.
```
这是因为编译器在编译的时候会将上面代码转换成如下代码：
```cpp
double a = 2;
const int temp = a;
const int &b = *temp;
```
这样会使b绑定到temp上，如果b不是const的话，如果编程人员想通过b修改a的值，他实际上修改的是temp的值，a没有修改，所以这个方式是被C++禁止的。

# pointer to const 和 const pointer
## pointer to const不能用来改变指向的变量的值，但是可以指向另外一个变量
```cpp
double a = 3.14;
double c = 3.1415;
const double *b = &a; // right.
b = &c; // right.
*b = 3.1; // wrong.
```

## 与reference不一样，pointer是object，所以它可以是const
const pointer必须在初始化，并且初始化之后就不能改变了，但是可以用它修改所指向的变量的值（如果变量是非const的话）。
我们表示const pointer的时候用*const来表示。
```cpp
int a = 2;
int *const p = &a; // right.
*p = 6; // right.
int c = 4;
p = &c; // wrong.
```

# top-level const 和low-level const
top-level const: the pointer itself is const.

low-level const: when a pointer can point to a const object, the const is low-level const.
