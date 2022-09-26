---
title: Physics Experiment
description: 模拟一堆球下落，听起来很难？
date: 2022-08-10T18:56:00+08:00
categories:
- 题解
tags:
- 物理
---

## 题面（英）

### Description

Simon is doing a physics experiment with $\small{N}$ identical balls with the same radius of $\small{R}$ centimeters. Before the experiment, all $\small{N}$ balls are fastened within a vertical tube one by one and the lowest point of the lowest ball is $\small{H}$ meters above the ground. At beginning of the experiment, (at second $\small{0}$), the first ball is released and falls down due to the gravity. After that, the balls are released one by one in every second until all balls have been released. When a ball hits the ground, it will bounce back with the same speed as it hits the ground. When two balls hit each other, they with exchange their velocities (both speed and direction).

Simon wants to know where are the $\small{N}$ balls after $\small{T}$ seconds. Can you help him?

In this problem, you can assume that the gravity is constant: $\small{g = 10m/s^2}$.

### Input

The first line of the input contains one integer $\small{C}$ ($\small{C \le 20}$) indicating the number of test cases. Each of the following lines contains four integers $\small{N}$, $\small{H}$, $\small{R}$, $\small{T}$.

$\small{1 \le N \le 100}$ \
$\small{1 \le H \le 10000}$ \
$\small{1 \le R \le 100}$ \
$\small{1 \le T \le 10000}$

### Output

For each test case, your program should output $\small{N}$ real numbers indicating the height in meters of the lowest point of each ball separated by a single space in a single line. Each number should be rounded to $\small{2}$ digit after the decimal point.

### Sample Input

```
2
1 10 10 100
2 10 10 100
```

### Sample Output

```
4.95
4.95 10.20
```

## 解法

首先简化一下问题，看一下只有一个球的情况。此时这只是单纯的物理问题，背公式：
$$t = \sqrt{\frac{2H}{g}}$$
求小球的高度：\
当$k$（满足$kt \le T$的最大整数）是偶数：
$$H - \frac{1}{2}g(T - kt) ^ 2$$
是奇数：
$$H - \frac{1}{2}g(kt + t - T) ^ 2$$