---
title: 哈希表
description: 在将近常数的时间中模拟集合。
date: 2022-04-21T21:24:00+08:00
categories:
- 数据结构
tags:
- 哈希表
---

## 功能

```c++
add(8);
add(9);
find(10); // false
find(8); // true
```

## 实现

首先，写一个散列函数。
```c++
int get(int x) {
    return (x % N + N) % N; // 解决负数问题
}
```
接着，就出现了问题，散列值可能发生碰撞，如何解决呢？

### 拉链法

每个散列值存一个链表（结构就很像邻接表了）。

代码如下：
```c++
#include <cstring>
#include <iostream>

using namespace std;

const int N = 100003;

int h[N], e[N], ne[N], idx;

void insert(int x) {
    int k = (x % N + N) % N;
    e[idx] = x;
    ne[idx] = h[k];
    h[k] = idx ++ ;
}

bool find(int x) {
    int k = (x % N + N) % N;
    for (int i = h[k]; i != -1; i = ne[i])
        if (e[i] == x)
            return true;

    return false;
}

int main() {
    int n;
    scanf("%d", &n);

    memset(h, -1, sizeof h);

    while (n -- ) {
        char op[2];
        int x;
        scanf("%s%d", op, &x);

        if (*op == 'I') {
            insert(x);
        } else {
            if (find(x)) puts("Yes");
            else puts("No");
        }
    }

    return 0;
}
```

### 开放寻址法

通过让N增大减少碰撞。
并在每次碰撞发生时向后找空位（这是放的时候）。
代码如下：
```c++
#include <cstring>
#include <iostream>

using namespace std;

const int N = 200003, null = 0x3f3f3f3f;

int h[N];

int find(int x) {
    int t = (x % N + N) % N;
    while (h[t] != null && h[t] != x)
    {
        t ++ ;
        if (t == N) t = 0;
    }
    return t;
}

int main() {
    memset(h, 0x3f, sizeof h);

    int n;
    scanf("%d", &n);

    while (n -- ) {
        char op[2];
        int x;
        scanf("%s%d", op, &x);
        if (*op == 'I') {
            h[find(x)] = x;
        } else {
            if (h[find(x)] == null) puts("No");
            else puts("Yes");
        }
    }

    return 0;
}
```
