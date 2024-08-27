---
title: GESP7级做题经验及题解
description: 包括2023-12,2024-03,2024-06
date: 2024-08-26T22:03:00+08:00
categories:
- 经验
tags:
- GESP
- 图论
- 树
- 数论
- 最短路
- 二分图
- 模拟
- 搜索
- DFS
- BFS
---

### 更新记录

[2024-08-26 22:03] 完成了 <商品交易> 和 <纸牌游戏> 的题解与感受。\
[2024-08-27 15:08] 完成了 <交流问题> 的题解与感受，并提供了所有题目的正确代码。

### 正确代码

[洛谷 - 云剪贴板](https://www.luogu.com.cn/paste/nxiws747)，[Github - Gist](https://gist.github.com/ray2060/3476b18baeceb672160b1388f31dcfea)

## 商品交易 202312 A

[洛谷题库 P10110](https://www.luogu.com.cn/problem/P10110)

### 题解

本质上是求一个边权为 $1$ 的最短路。每进行一次交换，总资产就会少 $1$ 元。

根据题意，用手上的第 $x$ 种商品交换第 $y$ 种商品，花费 $v_y - v_x + 1$ 元，总资产变化为 $-v_x + v_y - (v_y - v_x + 1) = -1$。（$-v_x$ 是第 $x$ 件商品没了，$+v_y$ 同理）

最后加上期望获得的商品的价格 $v_b$ 与原本持有的商品的价格 $v_a$ 的差即可。

### 感受

非常巧妙的一道最短路问题，算是一个简化版的 SPFA（由于边权为 $1$，所以不需要 `st` 数组防止重复入队）。

放段代码比较一下：

**SPFA 模板**
```cpp
int spfa() {
    memset(dist, 0x3f, sizeof dist);
    queue<int> q;
    dist[1] = 0; st[1] = true; q.push(1);
    while (!q.empty()) {
        int t = q.front(); q.pop();
        st[t] = false;
        for (int i = h[t]; i != -1; i = ne[i]) {
            int j = e[i];
            if (dist[j] > dist[t] + w[i]) {
                dist[j] = dist[t] + w[i];
                if (!st[j]) {
                    q.push(j); st[j] = true;
                }
            }
        }
    }
    if (dist[n] == INF) return INF;
    return dist[n];
}
```

**本题正解**
```cpp
int main() {
    /*
        ...
        v 数组有必要存的只有 v[a], v[b]
        所以用 va, vb 两个 int 替代
    */
    queue<int> q;
    q.push(a);
    st[a] = 1;
    dist[a] = 0;
    while (!q.empty()) {
        int t = q.front(); q.pop();
        if (t == b) {
            break;
        }
        for (int i = h[t]; ~i; i = ne[i]) {
            int j = e[i];
            if (dist[j] > dist[t] + 1) {
                dist[j] = dist[t] + 1;
                q.push(j);
            }
        }
    }
    if (dist[b] != INF) {
        cout << vb - va + dist[b] << endl;
    } else {
        cout << "No solution";
    }
    return 0;
}
```

数据量有点迷惑性，$10^5$ 对应的时间复杂度 $O(n\log n)$ 让人联想到 Dijkstra 等算法，然后发现实际上是一个 $O(n)$ 的 BFS。

此题难度不高，出题组第一次出七级的题可能把握不住题目难度，不作七级题目难度参考。

## 纸牌游戏 202312 B

[洛谷题库 P10111](https://www.luogu.com.cn/problem/P10111)

### 题解

经典最值问题，有非常浓厚的状态转移风味，并且无后效性，考虑 DP。

- 状态表示 `f[i][j][k]` 表示在第 $i$ 轮出了牌 $j$、累计换牌（额外扣分） $k$ 次时可以获得的最高分数。
- 状态计算 `f[i][j][k] = max(f[i - 1][t][k + ((j == t) ? 0 : -1)])`，其中 $t \in \lbrace 0, 1, 2 \rbrace$。

最后取所有 $j \in \lbrace 0, 1, 2 \rbrace , 0 \leq k \le n$ 的 `f[n][j][k] - b[k]` 的最大值（`b[k]` 为额外扣分数组的前缀和）。

注：可以把 `f` 数组的第一维压掉，但不差这点空间。

### 感受

> 做代码优化时，别的不用考虑，只要保证做的是**等价变形**。——yxc

DP 的状态表示基本靠硬想，唯一的技巧就是题里有啥就写啥，想到什么表示方法就试试，不行的话就尝试增加或减少一个维度之类的。

偶尔做题时会发现一些有意思的小事情，比如每轮得分可以表示为 `((c[i] == j) ? a[i] : (2 * ((c[i] + 3 - j) % 3 - 1) * a[i]))`（`c[i]` 为对手出牌，`j` 为我方出牌，`a[i]` 为本局分数）~~，然后你就会发现这玩意完全没用~~。

## 交流问题 202403 A

[洛谷题库 P10378](https://www.luogu.com.cn/problem/P10378)

### 题解

A 校和 B 校很明显是一个二分图的两部分。每个连通块都有两种分学校的方案，可以通过染色法分出学校，左右两半分别是 A B 校或 B A 校，两种方案产生了两组“该连通块中 A 校人数和 B 校人数”，记录两组中较小的 B 校人数（其实 B 校还是 A 校没区别，所以也可以只求一组，取两校人数的最小值）。仅需计算该二分图所有的连通块的“较小的 B 校人数”之和，该数是 B 校总人数的最小值，用 $n$ 减去该数就是 B 校总人数的最大值（即 A B 校学生数互换）。

### 感受

二分图明显就明显在所有边（交流）都在两学校之间，学校内不连边。每个连通块内的集合（学校）划分就是给每个点（学生）打标签，与染色法十分相似。

很水，感觉思考深度和前两题完全不是一个级别（话好像说反了）。

## 俄罗斯方块 202403 B

[洛谷题库 P10379](https://www.luogu.com.cn/problem/P10379)

待更新。

## 黑白翻转 202406 A

[洛谷题库 P10723](https://www.luogu.com.cn/problem/P10723)

待更新。

## 区间乘积 202406 B

[洛谷题库 P10724](https://www.luogu.com.cn/problem/P10724)

待更新。
