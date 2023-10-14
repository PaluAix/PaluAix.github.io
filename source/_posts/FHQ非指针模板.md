---
title: FHQ_Treap非指针模板
abbrlink: 63159
categories: 数据结构
date: 2023-10-14 19:42:31
---

# 【模板】普通平衡树

## 题目描述

您需要写一种数据结构（可参考题目标题），来维护一些数，其中需要提供以下操作：

1. 插入 $x$ 数
2. 删除 $x$ 数(若有多个相同的数，应只删除一个)
3. 查询 $x$ 数的排名(排名定义为比当前数小的数的个数 $+1$ )
4. 查询排名为 $x$ 的数
5. 求 $x$ 的前驱(前驱定义为小于 $x$，且最大的数)
6. 求 $x$ 的后继(后继定义为大于 $x$，且最小的数)

## 输入格式

第一行为 $n$，表示操作的个数,下面 $n$ 行每行有两个数 $\text{opt}$ 和 $x$，$\text{opt}$ 表示操作的序号( $ 1 \leq \text{opt} \leq 6 $ )

## 输出格式

对于操作 $3,4,5,6$ 每行输出一个数，表示对应答案

## 样例 #1

### 样例输入 #1

```
10
1 106465
4 1
1 317721
1 460929
1 644985
1 84185
1 89851
6 81968
1 492737
5 493598
```

### 样例输出 #1

```
106465
84185
492737
```

## 提示

【数据范围】  
对于 $100\%$ 的数据，$1\le n \le 10^5$，$|x| \le 10^7$

# 题解

```cpp
#include <bits/stdc++.h>
#define LC(p) t[p].c[0]
#define RC(p) t[p].c[1]
using namespace std;

const int N = 1e6 + 10;
struct FHQ_Treap {
  int val, size, dat, cnt, c[2];
} t[N];

int tot, root, a, b, c;

void Push(int p) { t[p].size = t[LC(p)].size + t[RC(p)].size + 1; }

void Split(int p, int k, int &a, int &b) {
  if (!p) {
    a = b = 0;
    return;
  }
  if (t[p].val <= k) {
    a = p;
    Split(RC(p), k, RC(p), b);
  } else {
    b = p;
    Split(LC(p), k, a, LC(p));
  }
  Push(p);
}

int Merge(int x, int y) {
  if (!x || !y) return x + y;
  if (t[x].dat <= t[y].dat) {
    RC(x) = Merge(RC(x), y);
    Push(x);
    return x;
  } else {
    LC(y) = Merge(x, LC(y));
    Push(y);
    return y;
  }
}

void Insert(int k) {
  t[++tot].val = k;
  t[tot].size = 1;
  t[tot].dat = rand();
  Split(root, k, a, b);
  root = Merge(Merge(a, tot), b);
}

void Remove(int k) {
  Split(root, k, a, b);
  Split(a, k - 1, a, c);
  c = Merge(LC(c), RC(c));
  root = Merge(Merge(a, c), b);
}

int GetRank(int k) {
  Split(root, k - 1, a, b);
  int ans = t[a].size + 1;
  root = Merge(a, b);
  return ans;
}

int GetVal(int p, int k) {
  if (k == t[LC(p)].size + 1) return t[p].val;
  if (k <= t[LC(p)].size)
    return GetVal(LC(p), k);
  else
    return GetVal(RC(p), k - t[LC(p)].size - 1);
}

int Pre(int p) { return GetVal(root, GetRank(p) - 1); }
int Nex(int p) { return GetVal(root, GetRank(p + 1)); }

int main() {
  int n;
  cin >> n;
  while (n--) {
    int opt, k;
    cin >> opt >> k;
    switch (opt) {
      case 1:
        Insert(k);
        break;
      case 2:
        Remove(k);
        break;
      case 3:
        cout << GetRank(k) << endl;
        break;
      case 4:
        cout << GetVal(root, k) << endl;
        break;
      case 5:
        cout << Pre(k) << endl;
        break;
      case 6:
        cout << Nex(k) << endl;
        break;
    }
  }
  system("pause");
  return 0;
}
```

码风自认为最好看的一次（笑

