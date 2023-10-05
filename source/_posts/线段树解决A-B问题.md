---
title: 线段树解决A+B问题
date: 2023-10-03 15:10:58
abbrlink: 6
categories: 数据结构
---

> 无意在洛谷上看见了自己A的第一道题A+B Problem，十分感慨。但可惜时间太过长久忘记了此题正确的解法，只好使出下策：线段树！

# A+B Problem

## 题目描述

输入两个整数 $a, b$，输出它们的和（$|a|,|b| \le {10}^9$）。

注意

1. Pascal 使用 `integer` 会爆掉哦！
2. 有负数哦！
3. C/C++ 的 main 函数必须是 `int` 类型，而且 C 最后要 `return 0`。这不仅对洛谷其他题目有效，而且也是 NOIP/CSP/NOI 比赛的要求！

好吧，同志们，我们就从这一题开始，向着大牛的路进发。

> 任何一个伟大的思想，都有一个微不足道的开始。

## 输入格式

两个以空格分开的整数。

## 输出格式

一个整数。

## 样例 #1

### 样例输入 #1

```
20 30
```

### 样例输出 #1

```
50
```



# 题解

```cpp
#include <bits/stdc++.h>
#define LC p << 1
#define RC p << 1 | 1
#define int long long
using namespace std;
const int N = 1e5;
inline int IN() {
  char c = getchar();
  int x = 0, f = 1;
  while (c < '0' || c > '9') {
    if (c == '-') f = -1;
    c = getchar();
  }
  while (c >= '0' && c <= '9') {
    x = x * 10 + c - '0';
    c = getchar();
  }
  return x * f;
}
struct AplusB {
  int l, r, val;
} t[N * 4];
void PushUp(int p) {
  t[p].val = t[LC].val + t[RC].val;
  return;
}
void Build(int p, int l, int r) {
  t[p].l = l, t[p].r = r;
  if (l == r) {
    t[p].val = 0;
    return;
  }
  int mid = (t[p].l + t[p].r) >> 1;
  Build(LC, l, mid);
  Build(RC, mid + 1, r);
  PushUp(p);
}
void add(int p, int x, int v) {
  if (t[p].l == t[p].r) {
    t[p].val += v;
    return;
  }
  int mid = (t[p].l + t[p].r) >> 1;
  if (x <= mid)
    add(LC, x, v);
  else
    add(RC, x, v);
  PushUp(p);
  return;
}
int Ask(int p, int l, int r) {
  if (t[p].l >= l && t[p].r <= r) return t[p].val;
  int ans = 0;
  int mid = (t[p].l + t[p].r) >> 1;
  if (l <= mid) ans += Ask(LC, l, r);
  if (r > mid) ans += Ask(RC, l, r);
  return ans;
}
signed main() {
  int a = IN(), b = IN();
  Build(1, 1, 1);
  add(1, 1, a), add(1, 1, b);
  printf("%lld", Ask(1, 1, 1));
  system("pause");
  return 0;
}

```

