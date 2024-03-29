---
title: 快速幂
description: 乘方的复杂度居然不是O(k)？
date: 2022-08-11T11:56:00+08:00
categories:
- 算法
tags:
- 位运算
- 快速幂
---

## 目的

求$m ^ k$ $\bmod$ $p$
## 反复平方法

### 预处理

$O(k)$预处理出来以下的数：\
$m ^ {2 ^ 0}$ $\bmod$ $p$\
$m ^ {2 ^ 1}$ $\bmod$ $p$\
$m ^ {2 ^ 2}$ $\bmod$ $p$\
...\
$m ^ {2 ^{\log{k}}}$ $\bmod$ $p$

怎么做？直接循环！代码忽略，因为还要优化。

### 算答案

举例子，$4 ^{5}$ $\bmod$ $10$
$ = (4^{2 ^ 2}$ $\bmod$ $10$ $\times$ $4 ^{2 ^ 0}$ $\bmod$ $10)$ $\bmod$ $10$

就可以调预处理的数了，\
$4^{2 ^ 2}$ $\bmod$ $10$ $ = $ $6$\
$4^{2 ^ 0}$ $\bmod$ $10$ $ = $ $4$\
$(6$ $\times$ $4)$ $\bmod$ $10$ $ = $ $4$

验证一下：\
$4^5$ $\bmod$ $10$ $=$ $1024$ $\bmod$ $10$ $=$ $4$

把$k$转化为二进制并取出对应的数再相乘，$O(k)$。

### 把两个步骤合在一起

```c++
int qmi(int m, int k, int p) {
    int t = 1 % p;
    while (k) {
        if (k & 1) t = (LL)t * m % p;
        m = (LL)m * m % p;
        k >>= 1;
    }
    return t;
}
```
关键操作：
- `k & 1` $k$ 的末位（二进制）为$0$
- `LL` 乘的过程可能会爆`int`，使用`long long`

至此，模板题就可以$\Large{\color{green}{AC}}$辣！

## 应用题

### 幂次方

#### 题面

对任意正整数 $N$，计算 $X^N \bmod 233333$ 的值。

（输入输出不用管他）

#### 解法

把$233333$看作$p$，彻头彻尾的一道快速幂题。

`qmi(X, N, 233333)`

### 越狱

#### 题面

监狱有连续编号为 $1$ 到 $n$ 的 $n$ 个房间，每个房间关押一个犯人。

有 $m$ 种宗教，每个犯人可能信仰其中一种。

如果相邻房间的犯人信仰的宗教相同，就可能发生越狱。

求有多少种状态可能发生越狱，对 $100003$ 取余。

#### 解法

直接想发生越狱的情况数不好想，那就想不发生越狱，$1$有$n$钟选择，$2$要想不发生越狱需要避开$1$的选择，有$n - 1$钟，后面的每一个人都需要避开前面一人的选择，都是$n - 1$种选择。列算式：
$$m (m - 1)^{n-1}$$
那不越狱的方案数就是：
$$m^n - m(m - 1)^{n - 1}$$
用快速幂解决问题。
