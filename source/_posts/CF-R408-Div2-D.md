---
title: Codeforces Round 408 Div2 Problem D
date: 2017-04-16 20:43:03
categories:
  - Competitive Programming
  - Codeforces
tags:
  - Codeforces
  - Tree
  - BFS
---

# [Police Station](http://codeforces.com/contest/796/problem/D)

## Solution sketch

直接從每個警察局直接 DFS 到距離 $d$ 後拔邊會GG。（可以試試看：第一筆 test case 在 $city\ 5$ 加一條邊到 $city\ 7$ ，跑跑看就懂了）

AC 解利用的性質是題解說的，$k - 1$條路會被拔掉，變成 $k$ 個 forests。做法是利用 BFS 從所有警局出發（注意多警局在一城市的問題），走到已經走訪過的城市的話，那條邊就可以拔掉。

P.S. $step > d$ 的理由，可以用下面的測資思考。概念上是說，在沒距離可用後的邊，也要檢查看看能不能拔掉。
```
6 6 0
1 2 3 4 5 6
1 2
2 3
3 4
4 5
5 6
```

<!-- more -->

## AC code

```c++
#include <bits/stdc++.h>

using namespace std;

typedef pair<int, int> ii;

vector<ii> g[300010];

int main()
{
    int n, k, d;
    scanf("%d %d %d", &n, &k, &d);

    // Can't come up with the solution using DFS QAQ
    queue<ii> q;
    int par[300010];
    memset(par, -1, sizeof(par));
    bool police[300010] = {false};
    for (int i = 0; i < k; i++) {
        int pos;
        scanf("%d", &pos);

        if (police[pos] == false) {
            q.push(ii(pos, 0));
            par[pos] = 0;
            police[pos] = true;
        }
    }

    for (int i = 1; i <= n - 1; i++) {
        int u, v;
        scanf("%d %d", &u, &v);
        g[u].push_back(ii(v, i));
        g[v].push_back(ii(u, i));
    }

    set<int> ans;
    while (q.empty() == false) {
        ii cur = q.front();
        int u = cur.first;
        int step = cur.second;
        q.pop();

        if (step > d)
            continue;

        for (auto v : g[u]) {
            if (v.first == par[u])
                continue;

            if (par[v.first] != -1) {
                ans.insert(v.second);
            } else {
                q.push(ii(v.first, step + 1));
                par[v.first] = u;
            }
        }
    }

    printf("%d\n", (int)ans.size());
    for (auto i : ans)
        printf("%d ", i);

    return 0;
}

```
