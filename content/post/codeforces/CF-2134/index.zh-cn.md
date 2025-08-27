---
title: CF-2134 (Codeforces Round 1045, Div. 2) 题解
description:
date: 2025-08-27T17:50:00+08:00
draft: true
categories:
- Codeforces
tags:
---

## [2134A. Painting With Two Colors](https://codeforces.com/contest/2134/problem/A)

### 题意

给定三个正整数 $n, a, b$。

有一排 $n$ 个格子，每个格子初始颜色为白色。你需要进行以下两步操作：

1. 选择 $a$ 个连续的格子，将这些格子涂成红色；
2. 选择 $b$ 个连续的格子，将这些格子涂成蓝色。

后涂上的颜色会覆盖先涂上的颜色。

请问是否存在一种涂色方案使得这一排格子是*左右对称的*？

### 题解

考虑更简单的问题
