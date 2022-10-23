---
title: 二分爆long long解决方法
description: 看二分代码总有一行不理解
date: 2022-10-23T14:07:00+08:00
categories:
- 经验
tags:
- 二分
---

## 爆掉出现的问题

此处使用爆`int`举例：
```cpp
#include <iostream>

using namespace std;

bool check(int x) {
    return x > 1919810;
}

int main() {
    int l = 114514, r = 2.14748e9;
    while ((r - l) > 1) {
        // 1.
        int mid = (l + r) / 2;
        // 2.
        int mid = l + (r - l) / 2;
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
结果是：

使用`1`号代码输出`-1073686390`，使用`2`号代码输出`1919810`。

## 为什么它等于mid

`l + (r - l) / 2`

这个表达式转换一下：$l + \frac{r - l}{2}$，
也就是$\frac{2 \times l + r - l}{2}$，
即$\frac{l + r}{2}$。

就是`l`和`r`的平均值`mid`。

## 为什么不会爆

首先，一般的二分答案都是二分正数（不是，答案范围一般不大，如浮点二分三次方根），那`r - l`就不会爆。

`r - l`不爆，`(r - l) / 2`也不会爆。

至于`l + (r - l) / 2`，很明显，这个值一定在`l`和`r`之间，既然`l`和`r`不爆，那这个就不可能爆。

## 总结

当然，除非真的要爆`long long`了才会用这招，不然写错了可就爆0了。

## 附：整型浮点型爆炸极限

|类型名|二分爆炸极限|EPS建议值|
|-----|-----|-----|
|int|l = 0, r = 1073741823|???|
|long long|l = 0, r = 4.6e18|???|
|double|???|1e-8（可存15位十进制）|
|long double|???|1e-26（可存33位十进制）|