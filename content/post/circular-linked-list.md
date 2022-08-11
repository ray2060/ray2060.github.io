---
title: 循环队列
description: 用数组构造循环队列。
date: 2022-04-19T15:25:00+08:00
categories:
- 算法
tags:
- queue
- 循环队列
---

## 循环队列

### 功能

节省空间，比如说：

操作：
```c++
push(1); pop(); push(2); pop();
```

效果：
hh以前的空间不能用了。

用循环队列可以把队列拉成数组的长度，而普通队列不一定。

### 代码

```c++
#include <iostream>

using namespace std;

// 队列长度，可以自己修改。
const int N = 10;

// 队列
struct Queue {
    int q[N];
    int hh = 1, tt = 0;

    void push(int x) {
        q[( ++ tt) % N] = x;
        if (hh / N && tt / N) {
            hh -= (int)min(hh / N, tt / N) * N;
            tt -= (int)min(hh / N, tt / N) * N;
        }
    }

    void pop() {
        hh ++ ;
    }

    int front() {
        return q[hh % N];
    }

    int back() {
        return q[tt % N];
    }

    bool empty() {
        return hh > tt;
    }

    int size() {
        return tt - hh + 1;
    }
};

// 调试
int main() {
    Queue q;
    q.push(1);
    q.pop();
    q.push(1);
    q.push(2);
    cout << (q.empty() ? 'Y' : 'N') << endl;
    q.push(3);
    q.pop();
    q.pop();
    q.pop();
    q.push(1);
    q.push(2);
    q.push(3);
    cout << q.front() << ' ';
    cout << q.back() << endl;
    q.push(4);
    q.push(5);
    cout << q.size();
    q.push(6);
    q.pop();
    q.pop();
    q.pop();
    q.pop();
    q.pop();
    q.pop();
    return 0;
}
```

## 版权问题

没有问题，背着抱着都行。

## 评论区问题

评论区不能抱走， sofa你是背不动滴，给我一句留言我就让你抱走。
