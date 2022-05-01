---
title: 题解-景区三角形
description: 手写数据结构的快乐就在这篇题解里。
date: 2022-05-01T21:07:00+08:00
categories:
- 题解
tags:
- 计蒜客
- graph
- queue

---

## 题目

题目链接：[景区三角形](https://nanti.jisuanke.com/t/T3633)

## 解法

### 暴力解

直接找出三个点，进行`check()`，非常简单的思路。

### 满分解

很明显，这是一个无向图，看数据范围可以看出应该用邻接表。

暴力解的缺点在于可能挑出三个毫无关联的点，那就让每个点拥有1个`fa`，有了它的存在，
我们就得找这个图的生成树，因为起码得有个点是根才有`fa`的概念。

假设遍历到的点是`cur`，找到相邻点`v`，那么我们只用看`fa`和`v`是否相邻了，变得非常简单，
复杂度降低了很多。

最终，再来谈论一下实现，在判断是否相邻时，使用二分查找，进一步优化。

放代码：
```c++
#include <iostream>
#include <algorithm>
#include <queue>
#include <vector>

using namespace std;

const int N = 1e5 + 10;

int n, m;
bool st[N];
int a, b, c;
vector<int> G[N];

struct Node {
    int x, fa;
};

int main() {
    //freopen("triangle.in", "r", stdin);
    //freopen("triangle.out", "w", stdout);
    cin >> n >> m;
    for (int i = 0; i < m; i ++ ) {
        int u, v;
        cin >> u >> v;
        G[u].push_back(v);
        G[v].push_back(u);
    }
    for (int i = 1; i <= n; i ++ ) {
        sort(G[i].begin(), G[i].end());
    }

    // bfs
    queue<Node> q;
    q.push(Node{1, -1});
    st[1] = 1;
    while (!q.empty()) {
        Node cur = q.front();
        q.pop();
        bool flag = false;
        for (int i = 0; i < G[cur.x].size(); i ++ ) {
            int v = G[cur.x][i];
            if (cur.fa != -1 && st[v] && v != cur.fa && binary_search(G[v].begin(), G[v].end(), cur.fa)) {
                a = cur.fa;
                b = cur.x;
                c = v;
                if (a > b) swap(a, b);
                if (b > c) swap(b, c);
                if (a > b) swap(a, b);
                flag = true;
                break;
            }
            if (!st[v]) {
                st[v] = true;
                q.push(Node{v, cur.x});
            }
        }
        if (flag) break;
    }
    printf("%d %d %d \n", a, b, c);
    return 0;
}
```

## 拓展：基本不使用STL完成这道题

### 手写邻接表

```c++
int h[N]; // 链表头
int e[M]; // 通往哪个点
int ne[M]; // 链表中下一个
int w[M]; // 边权
int idx; // 记录到第几条边

// 无权加边（有向）
void add(int a, int b) {
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

// 有权加边（有向）
void add(int a, int b, int c) {
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}
```

### 手写队列

```c++
int q[N], hh, tt;

// q.empty()
hh == tt

// q.push(x);
q[++ tt] = x;

// q.front()
q[hh + 1]

// q.pop();
hh ++ ;

// bfs模板
q[++ tt] = x;
st[x] = true;
while (hh != tt) {
    hh ++ ;
    if (check(q[hh])) {
        q[++ tt] = get(q[hh]);
    }
}
```

### 完整代码

注：抛弃了二分，否则太复杂了。
```c++
#include <iostream>
#include <cstring>

using namespace std;

const int N = 1e5 + 10;

int n, m;
bool st[N];
int a, b, c;
int h[N], e[N], ne[N], idx;
int q[N], fa[N], hh, tt;

void add(int u, int v) {
    e[idx] = v, ne[idx] = h[u], h[u] = idx ++ ;
}

bool find(int u, int v) {
    for (int i = h[u]; i != -1; i = ne[i]) {
        if (e[i] == v) return true;
    }
    return false;
}

void swap(int &x, int &y) {
    int z = x;
    x = y;
    y = z;
}

int main() {
    memset(h, -1, sizeof h);
    freopen("triangle.in", "r", stdin);
    freopen("triangle.out", "w", stdout);
    cin >> n >> m;
    for (int i = 0; i < m; i ++ ) {
        int u, v;
        cin >> u >> v;
        add(u, v);
        add(v, u);
    }

    q[++ tt] = 1;
    fa[tt] = -1;
    st[1] = 1;
    while (hh != tt) {
        hh ++ ;
        bool flag = false;
        for (int i = h[q[hh]]; i != -1; i = ne[i]) {
            int v = e[i];
            if (fa[hh] != -1 && st[v] && v != fa[hh] && find(fa[hh], v)) {
                a = fa[hh];
                b = q[hh];
                c = v;
                if (a > b) swap(a, b);
                if (b > c) swap(b, c);
                if (a > b) swap(a, b);
                flag = true;
                break;
            }
            if (!st[v]) {
                st[v] = true;
                q[++ tt] = v;
                fa[tt] = q[hh];
            }
        }
        if (flag) break;
    }

    printf("%d %d %d \n", a, b, c);
    return 0;
}
```
