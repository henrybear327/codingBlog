---
title: Codeforces Round 408 Div2 Problem C
date: 2017-04-16 17:06:03
categories:
  - Competitive Programming
  - Codeforces
tags:
  - Codeforces
  - Tree
  - Graph
---

# [Bank Hacking](http://codeforces.com/contest/796/problem/C)

## Solution sketch

跟 binary search 無關，還有，時間複雜度如果亂估會以為會 TLE 的好題。

這題[題解](http://codeforces.com/blog/entry/51527)寫得很清楚，我只補充一下我當初有另外想通的地方。

為何只有 $m$, $m+1$, 和 $m+2$ 三種可能答案呢？因為這其實是一個 tree。所以說，如果我們把樹吊起來想，對於每個node來說，他至多就只可能被自己的parent和parent's parent加到。

實作上，就是先建立一個總數表（我是對於$m+2$建表）。對於所有的點枚舉，只處理自己和他的鄰居這兩層後，求取minimax。

<!-- more -->

## AC code

```c++
#include <bits/stdc++.h>

using namespace std;

typedef pair<int, int> ii;
vector<int> g[300010];
int main()
{
    int n;
    scanf("%d", &n);

    int inp[n];
    for (int i = 0; i < n; i++)
        scanf("%d", &inp[i]);

    for (int i = 0; i < n - 1; i++) {
        int u, v;
        scanf("%d %d", &u, &v);
        u--;
        v--;

        g[u].push_back(v);
        g[v].push_back(u);
    }

    int ans = INT_MAX;
    map<int, int> cnt;
    for (int i = 0; i < n; i++) {
        cnt[inp[i] + 2]++;
    }

    for (int i = 0; i < n; i++) {
        stack<ii> s;

        // self
        cnt[inp[i]]++;
        cnt[inp[i] + 2]--;
        s.push(ii(inp[i], -1));
        s.push(ii(inp[i] + 2, 1));

        // neighbor
        for (auto v : g[i]) {
            cnt[inp[v] + 1]++;
            cnt[inp[v] + 2]--;
            s.push(ii(inp[v] + 1, -1));
            s.push(ii(inp[v] + 2, 1));
        }

        // update ans
        for (auto i = cnt.rbegin(); i != cnt.rend(); i++) {
            if (i->second != 0) {
                ans = min(ans, i->first);
                break;
            }
        }

        // recover
        while (s.empty() == false) {
            ii cur = s.top();
            s.pop();

            cnt[cur.first] += cur.second;
        }
    }

    printf("%d\n", ans);

    return 0;
}

```
