---
title: 树状数组及典型例题
description: Fenwick Tree / Binary Indexed Tree (BIT)
date: 2025-07-13T11:40:00+08:00
categories:
- 数据结构
tags:
- 树
- 树状数组
---

> BIT 中 Binary 是形容 Index 的，不是形容 Tree 的。

## 树状数组

树状数组是一种支持**单点修改**和**区间查询**的代码量较小的数据结构。树状数组能解决的问题是线段树能解决的问题的子集，也就是说树状数组能解决的问题线段树一定能解决，线段树能解决的问题树状数组不一定能解决。

常规情况下用树状数组的前提条件：存储的信息和运算方法要支持**结合律**和**可差分**。

> 可差分：有逆运算。

当然，例如 $\max$ 等不可差分运算，树状数组只能支持**前缀查询**。

### 引入

举个例子，我们想知道 $a[1] + a[2] + \cdots + a[7]$ 的前缀和。

普通的做法是：直接将 $7$ 个数求和。

树状数组的做法是：将一段前缀 $[1, n]$ 分割成不超过 $\log n$ 段区间，并通过一些预处理使得每段区间的和都可以 $O(1)$ 地求出。于是，我们就成功地缩减了问题规模。

### 管辖区间

下图展示了树状数组的工作原理：

![](./assets/ds003-fenwick.svg)

不难发现，$c[i]$ 管辖 $[i - lowbit(i) + 1, i]$ 这段区间。

> `lowbit(x)` 是指 `x` 在二进制下最低的 `1` 及其后面的 `0`，根据二进制反码相关知识可以推导出较为简单的计算方法：`lowbit(x) = x & -x`。

### 前缀（区间）查询

为了不重不漏的拆分前缀区间，每次向前“跳”的时候一定是要“跳”到当前区间的左端点的左一位作为新区间的右端点，根据刚刚提到的管辖区间可得新右端点应为 $i - lowbit(i)$。

```cpp
int query(int x) {
    int res = 0;
    for (int i = x; i >= 1; i -= lowbit(i)) res += c[i]; // 此处运算方法根据题意替换；看个人习惯，for循环可写作while循环
    return res;
}
```

> 注意第 3 行不要写成 `i -= lowbit(x)`

根据前缀和的原理，只要运算方法是可差分的，可以前缀查询就是可以区间查询。

### 单点修改

在进行单点修改时，只需遍历所有管辖 $a[i]$ 的 $c[x]$。由于 $c[i]$ 被 $c[i + lowbit(i)]$ 直接管辖，我们就可以直接通过这种方式来遍历所有需要修改的 $c[x]$。

```cpp
void update(int x, int val) {
    for (int i = x; i <= n; i += lowbit(i)) c[i] += val; // 根据题意替换；可写作while循环
}
```

### 建树

最简单的方法是直接替换成 $n$ 次单点修改，复杂度 $O(n \log n)$。

也可以 $\Theta (n)$ 建树，方法很多，这里介绍一种：

既然 $c[i]$ 表示的区间是 $[i - lowbit(i) + 1, i]$，我们就可以 $\Theta (n)$ 建立一个前缀和数组，再通过前缀和求区间和的方法写入 $c$ 数组。

### 复杂度分析

时间复杂度：

- 建树：$\Theta (n)$ 或 $O(n \log n)$

- 修改：$O(\log n)$

- 查询：$O(\log n)$

空间复杂度：$O(n)$

### 参考代码

[P3374 【模板】树状数组 1](https://www.luogu.com.cn/problem/P3374)

```cpp
#include <iostream>

using namespace std;

typedef long long LL;

const int N = 5e5 + 10;

LL a[N], c[N];
int n, m;

inline int lowbit(int x) { return x & -x; }

void update(int x, LL val) {
    while (x <= n) {
        c[x] += val;
        x += lowbit(x);
    }
}

LL query(int x) {
    LL res = 0;
    while (x > 0) {
        res += c[x];
        x -= lowbit(x);
    }
    return res;
}

int main() {
    cin >> n >> m;
	for (int i = 1; i <= n; i ++ ) cin >> a[i], update(i, a[i]);

    while (m -- ) {
        int op;
        cin >> op;
        if (op == 1) {
            int x; LL k;
            cin >> x >> k;
            update(x, k);
        } else if (op == 2) {
            int x, y;
            cin >> x >> y;
            cout << query(y) - query(x - 1) << endl;
        }
    }
    return 0;
}
```

## Luogu P3368. 树状数组 2

[P3368 【模板】树状数组 2](https://www.luogu.com.cn/problem/P3368)

### 题意

区间修改，单点查询。

### 分析

我们可以通过差分算法将区间修改转化为单点修改，同时单点查询变为区间查询，树状数组秒了。

### 参考代码

```cpp
#include <iostream>

using namespace std;

typedef long long LL;

const int N = 5e5 + 10;

LL a[N], s[N];
LL c[N];
int n, m;

inline int lowbit(int x) { return x & -x; }

void update(int x, LL val) {
    while (x <= n) {
        c[x] += val;
        x += lowbit(x);
    }
}

LL query(int x) {
    LL res = 0;
    while (x > 0) {
        res += c[x];
        x -= lowbit(x);
    }
    return res;
}

int main() {
    cin >> n >> m;
	
	for (int i = 1; i <= n; i ++ ) cin >> a[i];
	for (int i = 1; i <= n; i ++ ) s[i] = a[i] - a[i - 1];
    for (int i = 1; i <= n; i ++ ) {
        update(i, s[i]);
    }

    while (m -- ) {
        int op;
        cin >> op;
        if (op == 1) {
            LL x, y, k;
            cin >> x >> y >> k;
            update(x, k);
            update(y + 1, -k);
        } else if (op == 2) {
            int x;
            cin >> x;
            cout << query(x) << endl;
        }
    }
    return 0;
}
```

## 未完待续 2025/07/21
