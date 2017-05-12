---
title: Codeforces Round 413 Division 2
categories:
  - Competitive Programming
  - Codeforces
  - Division 2
date: 2017-05-12 14:02:32
tags:
  - Codeforces
  - Math
---

# [Playrix Codescapes Cup (Codeforces Round #413, rated, Div. 1 + Div. 2)](http://codeforces.com/contest/799/)

A 不錯的數學題，但是不需要推公式就可做了... :)

<!-- more -->

## Solution sketch

這題剛開始幾次 submissions 都嘗試使數學公式解，然後就WA了一串 @@ 超級丟臉的。

後來 AC 做法是，開兩個計時器，一個算原本烤箱的，一個算新烤箱的。用 greedy 的思維，讓兩個烤箱時間差不超過 $t$ 的情況下，輪流工作即可。 哭哭歐...

## AC code

```c++
#include <bits/stdc++.h>

using namespace std;

int main()
{
    int n, t, k, d;
    scanf("%d %d %d %d", &n, &t, &k, &d);

    int times = (n / k) + (n % k == 0 ? 0 : 1);

    int ans1 = t * times;

    int ans2 = 0;
    int t1 = 0, t2 = d;
    for (int i = 0; i < times; i++) {
        if (t1 <= t2) {
            t1 += t;
        } else {
            t2 += t;
        }
    }
    ans2 = max(t1, t2);

    printf("%s\n", ans1 <= ans2 ? "NO" : "YES");

    return 0;
}

```
