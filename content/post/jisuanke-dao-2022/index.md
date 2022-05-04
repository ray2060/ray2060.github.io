---
title: 2022计蒜之道初赛（第一场）
description:
date: 2022-05-03T17:17:51+08:00
categories:
- 参赛经历
tags:
- 计蒜客
image: dao-2022.png
---

## 比赛简介

- 题数：4道
- 时间：2个小时
- 总分：400分
- 难度：和CSP-J2难度相当

## 开始考试

先来讲题。

A题签到题忽略……

### B：完美数列

题目链接：[完美数列](https://nanti.jisuanke.com/t/T3631)

#### 暴力解法

直接找最大值砍下去。

#### 60分解法（实际90分）

使用`priority_queue`来找最大值，一层一层砍。

#### 满分解

二分答案法，非常简洁！

```c++
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 1e6 + 10;

int n, p;
LL w, s[N];
int a[N];

bool check(int x) {
    int idx = upper_bound(a, a + n, x) - a;
    LL len = n - idx;
    LL tmp = s[idx] - len * x;
    return tmp <= w / p;
}

int main() {
    freopen("money.in", "r", stdin);
    freopen("money.out", "w", stdout);
    cin >> n >> p >> w;
    for (int i = 0; i < n; i ++ ) cin >> a[i];
    sort(a, a + n);
    for (int i = n - 1; i >= 0; i -- ) {
        s[i] = s[i + 1] + a[i];
    }
    int l = 0, r = 1e9 + 10;
    while (l < r) {
        int mid = (l + r) >> 1;
        if (check(mid)) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    cout << l << endl;
    return 0;
}
```

### C：传递

题目链接：[传递](https://nanti.jisuanke.com/t/T3632)

#### 40分

冒泡排序，边排边计算代价，非常暴力……

#### 满分解

这是带权逆序对问题。

所以通过归并求逆序对，从$p_2$移到$p_1$需要耗费代价

$((p_2 + mid)^2 + (p_2 + mid - 1)^2 + ... + (p_2 + p_1)^2) + (2 * w[mid] * w[p_2] + 2 * w[mid - 1] * w[p_2] + ... + 2 * w[p_1] * w[p_2]) + (mid - p_1 + 1) * w[p_2]$

然后，维护$w[i]$和$w[i] ^ 2$的后缀和就OK了。

```c++
#include <iostream>

using namespace std;

typedef long long LL;

const int N = 5e5 + 10, P = 1e9 + 7;

int n;
int w[N], p[N];
int a[N], b[N];
LL sum1[N], sum2[N];
LL ans;

void merge(int l, int r) {
    int mid = (l + r) >> 1;
    int p1 = l, p2 = mid + 1;
    int m = l;
    sum1[mid + 1] = sum2[mid + 1] = 0;
    for (int i = mid; i >= l; i -- ) {
        sum1[i] = (sum1[i + 1] + w[i]) % P;
        sum2[i] = (sum2[i + 1] + ((LL)w[i] * w[i]) % P) % P;
    }
    while (p1 <= mid && p2 <= r) {
        if (p[p1] <= p[p2]) {
            b[m] = w[p1];
            a[m ++ ] = p[p1 ++ ];
        } else {
            ans = (ans + sum2[p1]) % P;
            ans = (ans + 2 * sum1[p1] * w[p2] % P) % P;
            ans = (ans + (LL)(mid - p1 + 1) *  w[p2] * w[p2] % P) % P;
            b[m] = w[p2];
            a[m ++ ] = p[p2 ++ ];
        }
    }
    while (p1 <= mid) {
        b[m] = w[p1];
        a[m ++ ] = p[p1 ++ ];
    }
    while (p2 <= r) {
        b[m] = w[p2];
        a[m ++ ] = p[p2 ++ ];
    }
    for (int i = l; i <= r; i ++ ) {
        p[i] = a[i];
        w[i] = b[i];
    }
}
void msort(int l, int r) {
    if (l == r) {
        return;
    }
    int mid = (l + r) >> 1;
    msort(l, mid);
    msort(mid + 1, r);
    merge(l, r);
}

int main() {
    freopen("transfer.in", "r", stdin);
    freopen("transfer.out", "w", stdout);
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++ ) {
        scanf("%d", &w[i]);
    }
    for (int i = 1; i <= n; i ++ ) {
        scanf("%d", &p[i]);
    }
    msort(1, n);
    printf("%lld", ans);
    return 0;
}
```

### D：景区三角形

题目链接：[景区三角形](https://nanti.jisuanke.com/t/T3633)

#### 暴力解法

就$O(n^3)$找三个点，做个`check()`，就结束了，不再赘述。

#### 满分解

通过`bfs`找三角形，一个点设一个$fa$，根（1）等于$-1$。

有点像找生成树。

找到一个点$v$，判断$fa$，$cur$，$v$是否构成三角形，如果是，排序放进答案里输出，如果$v$没遍历过，遍历$v$。

这道题我写了更详细的题解（代码也包装进去了，这里不放了哈）：
[题解-景区三角形]({{< ref "sol-jisuanke-t3633" >}})

## 总结

### 不能放弃，记得调试

C题本来能40分的，但是没调试，放弃了，代码都没存档……

### 注意亿些关于输入输出的低级问题

赛后调试一道题时发现有问题，结果，没想到，`freopen`忘记注释了……

### 要用草稿纸

不用草稿纸基本只能做签到题，有些考试中的B题可能也不难，也能做出来。
关于C题的推导其实很长这件事……

### 想算法时的方法很重要

签到题如果复杂度不超标就行，这回也就简简单单一个快速幂。

其他题可以由复杂度推出算法，或是从暴力法开始优化，都可以。
