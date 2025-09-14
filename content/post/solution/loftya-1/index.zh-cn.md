---
title: Loftya 的题单题解 (1)
description: U609517 U609469 U606373
date: 2025-09-14T14:35:00+08:00
categories:
- 题解
tags:
---

## U609517 异或跳

<details>
<summary>提示</summary>
$x \oplus x = 0$
</details>

<details>
<summary>题解</summary>
容易发现，无论怎么跳，成本的异或和均为 $a_1 \oplus a_n$，直接输出即可。
</details>

## U609469 构造回文串

<details>
<summary>提示1</summary>
区间 DP
</details>

<details>
<summary>提示2</summary>
状态表示：$f[i][j]$ 表示令 $i$ 到 $j$ 的范围内为回文串的最小插入次数。
</details>

<details>
<summary>题解</summary>
区间 DP，状态表示：$f[i][j]$ 表示令 $i$ 到 $j$ 的范围内为回文串的最小插入次数。

状态转移：人人为我思路。

- $f[i + 1][j - 1]$ 可以转移到 $f[i][j]$ 当且仅当 $s[i] = s[j]$；

- $f[i + 1][j] + 1$ 可以转移到 $f[i][j]$ 无论何时；

- 同理 $f[i][j - 1] + 1$ 可以转移。
</details>

## U606373 油量问题

<details>
<summary>提示</summary>
贪心
</details>

<details>
<summary>题解</summary>
将位置 $0$ 视为加油站，在每个加油站选择最远能到达的加油站。
</details>
