---
title: "C++ sorting and tracking of indexes"
layout: post
category: [疑难杂症]
tags: [c++, algorithm]
excerpt: "做题遇到一个小问题，如何解决C++ 对vector排序并返回索引。"
---

# 问题来源

Pancake Glutton ([Beginner Exercises])

Requires: variables, data types, and numerical operators,
basic input/output,
logic (if statements, switch statements),
loops (for, while, do-while),
arrays

Write a program that asks the user to enter the number of pancakes eaten for breakfast by 10 different people (Person 1, Person 2, ..., Person 10)
Once the data has been entered the program must analyze the data and output which person ate the most pancakes for breakfast.

★ Modify the program so that it also outputs which person ate the least number of pancakes for breakfast.

★★★★ Modify the program so that it outputs a list in order of number of pancakes eaten of all 10 people.
i.e.
Person 4: ate 10 pancakes
Person 3: ate 7 pancakes
Person 8: ate 4 pancakes
...
Person 5: ate 0 pancakes

# 解决方法

用lambda

```cpp
template <typename T>
vector<size_t> sort_indexes(const vector<T> &v) {

  // initialize original index locations
  vector<size_t> idx(v.size());
  iota(idx.begin(), idx.end(), 0);

  // sort indexes based on comparing values in v
  sort(idx.begin(), idx.end(),
       [&v](size_t i1, size_t i2) {return v[i1] < v[i2];});

  return idx;
}
```

来源：[stackoverflow]

# 原始问题解决答案

```cpp
int pancakeGlutton()
{
    vector<int> pancakes;
    
    for (int i = 0; i < 10; i++) {
        cout << "How many pancakes did you eat?" << endl;
        int num;
        cin >> num;
        pancakes.push_back(num);
    }
    
    vector<size_t> idx(pancakes.size());
    iota(idx.begin(), idx.end(), 0);
    
    sort(idx.begin(), idx.end(),
         [&](size_t i1, size_t i2) {return pancakes[i1] > pancakes[i2];});
    
    for (auto& i : idx)
        cout << "The person " << i + 1 << " : ate " << pancakes[i] << " pancakes." << endl;
    
    return 0;
}
```



[Beginner Exercises]: http://www.cplusplus.com/articles/N6vU7k9E/
[stackoverflow]: https://stackoverflow.com/questions/1577475/c-sorting-and-keeping-track-of-indexes





