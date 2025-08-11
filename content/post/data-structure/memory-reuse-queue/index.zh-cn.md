---
title: 循环队列
description: 用数组构造循环队列。
date: 2022-04-19T15:25:00+08:00
categories:
- 数据结构
tags:
- 队列
- 循环队列
- 空间复杂度
---

> UPD: 2023-10-21
> UPD: 2025-07-13

## 循环队列

### 功能

节省空间。

普通队列用过的空间不能用了，循环队列可以重复利用。

```c++
push(1); pop(); push(2); pop();
```

实际比赛中根本用不到（现在这年头还有谁手写数据结构啊），直接用 STL queue。

### 代码

```c++
#include <iostream>

using namespace std;

// 队列长度，可以自己修改。
const int N = 10;

// 队列
struct Queue {
    int q[N];
    int hh = 1, tt = 0;

    void push(int x) {
        q[( ++ tt) % N] = x;
        if (hh / N && tt / N) {
            hh -= (int)min(hh / N, tt / N) * N;
            tt -= (int)min(hh / N, tt / N) * N;
        }
    }

    void pop() {
        hh ++ ;
    }

    int front() {
        return q[hh % N];
    }

    int back() {
        return q[tt % N];
    }

    bool empty() {
        return hh > tt;
    }

    int size() {
        return tt - hh + 1;
    }
};
```
