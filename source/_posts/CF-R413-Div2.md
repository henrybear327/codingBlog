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
  - Implementation
---

# [Playrix Codescapes Cup (Codeforces Round #413, rated, Div.1 + Div.2)](http://codeforces.com/contest/799/)

A 不錯的數學題，但是不需要推公式就可做了... :)
B 不錯的思考題，但是實作QAQ

<!-- more -->

## A

### Solution sketch

這題剛開始幾次 submissions 都嘗試使數學公式解，然後就WA了一串 @@ 超級丟臉的。

後來 AC 做法是，開兩個計時器，一個算原本烤箱的，一個算新烤箱的。用 greedy 的思維，讓兩個烤箱時間差不超過 $t$ 的情況下，輪流工作即可。 哭哭歐...

### AC code

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

## B

### Solution sketch

基本上按照前後的顏色，分別針對價錢去做排序。 對於每個用戶的需求，去找對應顏色的最低價。

比價的部份實作需要小心。

### AC code

```c++
#include <bits/stdc++.h>

using namespace std;

const int MAX_N = 200100;

struct Data {
    int price;
    int idx;

    Data(int price, int idx)
    {
        this->price = price;
        this->idx = idx;
    }

    bool operator<(const Data &other) const
    {
        return this->price > other.price;
    }
};

bool inStock[MAX_N];
priority_queue<Data> f[3], b[3];

int solve(int c)
{
    c--;

    Data candf(-1, -1), candb(-1, -1);

    // 找前面最低價
    while (f[c].size() > 0) {
        Data tmp = f[c].top();
        f[c].pop();

        if (inStock[tmp.idx] == false)
            continue;

        inStock[tmp.idx] = false;
        candf = tmp;
        break;
    }

    // 找後面最低價
    while (b[c].size() > 0) {
        Data tmp = b[c].top();
        b[c].pop();

        if (inStock[tmp.idx] == false)
            continue;

        inStock[tmp.idx] = false;
        candb = tmp;
        break;
    }

    if (candf.price == -1 || candb.price == -1) { // 只有一種顏色有得賣
        if (candf.price == -1) {
            return candb.price;
        } else {
            return candf.price;
        }
    } else if (candf.price == -1 && candb.price == -1) { // 此顏色無貨
        return -1;
    } else { // 比價
        if (candf.price <= candb.price) {
            if (candf.idx != candb.idx) { // 輸者塞回貨架中
                b[c].push(candb);
                inStock[candb.idx] = true;
            }
            return candf.price;
        } else {
            if (candf.idx != candb.idx) { // 輸者塞回貨架中
                f[c].push(candf);
                inStock[candf.idx] = true;
            }
            return candb.price;
        }
    }
}

int main()
{
    int n;
    scanf("%d", &n);

    int pr[n];
    int fc[n];
    int bc[n];
    for (int i = 0; i < n; i++)
        scanf("%d", &pr[i]);
    for (int i = 0; i < n; i++)
        scanf("%d", &fc[i]);
    for (int i = 0; i < n; i++)
        scanf("%d", &bc[i]);

    fill(inStock, inStock + MAX_N, 1);
    for (int i = 0; i < n; i++) {
        int c = fc[i];
        c--;
        f[c].push(Data(pr[i], i));

        c = bc[i];
        c--;
        b[c].push(Data(pr[i], i));
    }

    int m;
    scanf("%d", &m);
    for (int i = 0; i < m; i++) {
        int c;
        scanf("%d", &c);

        printf("%d%c", solve(c), i == m - 1 ? '\n' : ' ');
    }

    return 0;
}
```
