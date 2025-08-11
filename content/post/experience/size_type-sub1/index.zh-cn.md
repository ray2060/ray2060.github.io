---
title: size_t相当于unsigned long long
description: 没想到写个循环也能写出大道理
date: 2022-08-11T14:38:00+08:00
categories:
- 经验
tags:
- 数据类型
---

> UPD: 2025-07-15

## 问题

### 代码

```c++
vector<int> vec;
// some code here
for (int i = 0; i < vec.size() - 1; i ++ ) {
    cout << vec[i] << ' ';
}
```

### 原本思路

只打印$vec$的前$|vec| - 1$个元素，就用这个挺好！

### 显示出的问题

<font color="#ee0000">Segmentation Fault</font>
<font color="#ee0000">11</font>/<font color="#00ee00">0</font>/11

### 解读

```
C:/xxx/xxx.cpp:12:23: warning: comparison of integer expressions of different signedness: 'int' and 'std::vector<int>::size_type' {aka 'long long unsigned int'} [-Wsign-compare]
     for (int i = 0; i < vec.size(); i ++ ) {
```

可以发现如果`size`是$0$，又因为 `size_type` 是 `unsigned long long`，那么就会减出$18446744073709551615$，明显是要报错的。
