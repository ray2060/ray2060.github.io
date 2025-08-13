---
title: CF-2131 (Codeforces Round 1042, Div. 3) Solution
description: Zero delta predicted. Are you kidding me?
date: 2025-08-11T12:15:00+08:00
image: https://codeforces.org/s/15422/images/codeforces-sponsored-by-ton.png
categories:
- 题解
tags:
---

## [2131A. Lever](https://codeforces.com/contest/2131/problem/A)

Notice that the step $2$ does not affect step $1$.

For each $i$, the maximum number of times step $1$ can be performed is $\max(a_i - b_i, 0)$. So we can just sum up the number of times step $1$  is performed.

[Submission #333289337](https://codeforces.com/contest/2131/submission/333289337)

## [2131B. Alternating Series](https://codeforces.com/contest/2131/problem/B)

First, observe that every adjacent pair has different signs and every element is non-zero.

Then, let us try to minimize the sum of a subarray. It must have a length of $3$ and the first and third elements must be negative. So the sum is $a_{mid} - 2$. But we need that $a_{mid - 1} + a_{mid} + a_{mid + 1} \geq 1$. So we have $a_{mid} = 3$.

Now, let us try to construct the array from left to right. We can see that $a_1 = -1$ because we cannot pick $1$ due to the previous condition and we need to minimize the absolute value. Keep going, for every even index $i$, we'll pick $a_i = 3$, except for $i = n$ where we pick $2$. For every odd index $i$, we'll pick $a_i = -1$.

[Submission #333498392](https://codeforces.com/contest/2131/submission/333498392)
