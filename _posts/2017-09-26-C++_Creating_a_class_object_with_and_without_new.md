---
title: "C++ Creating a class object with and without new"
layout: post
category: [人生经验]
tags: [C++]
excerpt: "在做leetcode的一个题的时候，遇到了创建object的用new分配dynamic memory和不用new分配automatic memory的两个方法，查了stackoverflow上的答案，本文比较一下两种方法。"
---

问题来源：[leetcode: copy list with random pointer]

这个问题比较基本，简单来说就是

不用new分配的是automatic memory，在离开了作用域```cpp{}```后自动释放；

用new分配的是dynamic memory，必须人工delete，否则会造成内存泄漏。

在问题来源中答案是用new在for循环中创建object的，这是正确的，因为如果用automatic memory创建object，在离开for循环后会自动释放，这样就不符合创建的意思了。

下面是自己写的test代码：
```cpp
include <iostream>
#include <list>

using namespace std;

struct RandomListNode {
    int label;
    RandomListNode *next, *random;
    RandomListNode(int x) : label(x), next(NULL), random(NULL) {}
};

int main() {
    RandomListNode dummy(-1), *head = &dummy, *cur = head;
    for (int i = 0; i < 5; i++) {
        RandomListNode x(i);
        cur->next = &x;
        cur = cur->next;
    }
    cur->next = nullptr;

    for (auto x = head; x != nullptr; x = x->next) {
        cout << x->label << endl;
    }

    return 0;
}
```

代码中用的是automatic memory创建的object，打印的东西并不是[-1,0,1,2,3,4]，而是[-1,4]，证明中间的object在离开作用域之后就释放了，所以此处应该用new创建object。

在 stackoverflow中查了几个回答，有的说的比较明白，这两个方法没有哪个是preferred，应该看情况用，但是能用automatic memory的就先不用dynamic memory。

stackoverflow回答：

[Creating an object: with or without 'new']

[Why does the use of 'new' cause memory leaks?]

[leetcode: copy list with random pointer]: https://leetcode.com/problems/copy-list-with-random-pointer/description/
[Creating an object: with or without 'new']: https://stackoverflow.com/questions/6337294/creating-an-object-with-or-without-new
[Why does the use of 'new' cause memory leaks?]: https://stackoverflow.com/questions/8839943/why-does-the-use-of-new-cause-memory-leaks

