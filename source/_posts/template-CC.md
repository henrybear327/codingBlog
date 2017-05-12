---
title: (Template) Connected component
categories:
  - Competitive Programming
  - Template
  - Graph
date: 2017-05-03 01:04:55
tags:
  - Template
  - SCC
  - BCC
  - POJ
---

BCC and SCC，使用 Tarjan 來求取。

<!-- more -->

# BCC

一張無向圖上，不會產生關節點 (articulation point) 的連通分量，稱作「雙連通分量」(Biconnected Component)。

一張無向圖上，不會產生橋 (bridge) 的連通分量，稱作「橋連通分量」(Bridge-connected Component)。

## Bridge-connected Component 模板

```c++
const int MAX_N = 5555;
vector<int> g[MAX_N];

int tt, dfn[MAX_N], low[MAX_N];
int bcc;
int belong[MAX_N]; // 縮點用
stack<int> s;

void dfs(int u, int p)
{
    dfn[u] = low[u] = tt++;
    s.push(u);

    for (int i = 0; i < (int)g[u].size(); i++) {
        int v = g[u][i];

        if (v == p)
            continue;
        if (dfn[v] == -1) {
            dfs(v, u);
            low[u] = min(low[u], low[v]);
        } else {
            low[u] = min(low[u], dfn[v]);
        }
    }

    if (low[u] == dfn[u]) {
        bcc++;
        while (1) {
            int v = s.top();
            s.pop();
            belong[v] = bcc;

            if (v == u)
                break;
        }
    }
}
```

## [Biconnected Component 模板](http://www.geeksforgeeks.org/biconnected-components/)

stack 塞入邊而不是塞入點，這樣就可以在找到 articulation point 時，得到每個團的資訊。

## 驗證：[POJ 3177 Redundant Paths](http://poj.org/problem?id=3177)

求取 BCC 後縮點。縮點後最遠的兩葉子相連可以形成一個環，這需要做 (left + 1) / 2次。可以參考照篇[文章](http://www.cnblogs.com/frog112111/p/3367039.html)來思考。

注意重邊。

```c++
#include <algorithm>
#include <cstdio>
#include <cstring>
#include <stack>
#include <vector>
using namespace std;

const int MAX_N = 5555;
vector<int> g[MAX_N];

int tt, dfn[MAX_N], low[MAX_N];
int bcc;
int belong[MAX_N];
stack<int> s;
bool seen[MAX_N][MAX_N];

void dfs(int u, int p)
{
    dfn[u] = low[u] = tt++;
    s.push(u);

    for (int i = 0; i < (int)g[u].size(); i++) {
        int v = g[u][i];

        if (v == p)
            continue;
        if (dfn[v] == -1) {
            dfs(v, u);
            low[u] = min(low[u], low[v]);
        } else {
            low[u] = min(low[u], dfn[v]);
        }
    }

    if (low[u] == dfn[u]) {
        bcc++;
        while (1) {
            int v = s.top();
            s.pop();
            belong[v] = bcc;

            if (v == u)
                break;
        }
    }
}

int main()
{
    int n, m;
    scanf("%d %d", &n, &m);

    // memset(seen, 0, sizeof(seen)); // memset will cause MLE
    for (int i = 0; i < m; i++) {
        int u, v;
        scanf("%d %d", &u, &v);

        if (seen[u][v])
            continue;
        seen[u][v] = seen[v][u] = true;
        g[u].push_back(v);
        g[v].push_back(u);
    }

    memset(dfn, -1, sizeof(dfn));
    memset(low, -1, sizeof(low));
    memset(belong, 0, sizeof(belong));
    bcc = 0;
    tt = 0;
    for (int i = 1; i <= n; i++)
        if (dfn[i] == -1) {
            dfs(i, -1);
        }

    int degree[MAX_N];
    memset(degree, 0, sizeof(degree));
    for (int i = 1; i <= n; i++) {
        for (int j = 0; j < (int)g[i].size(); j++) {
            int v = g[i][j];

            if (belong[i] != belong[v]) {
                degree[belong[v]]++;
                degree[belong[i]]++;
            }
        }
    }

    int ans = 0;
    for (int i = 1; i <= n; i++) {
        if (degree[i] == 2)
            ans++;
    }
    printf("%d\n", (ans + 1) / 2);

    return 0;
}

```

# SCC

## 模板

in_stack 檢查可以用下圖思考。

$$
A \rightarrow B \\
C \rightarrow B \\
D \rightarrow C
$$

如果今天走訪順序為 $B, A, D$，沒檢查 in_stack 的話，$C$ 的 low 會變成 $B$ 的，造成 $C$ 和 $D$ 在相同 SCC，但是這是錯誤的。

```c++
const int MAX_N = 11111;
vector<int> g[MAX_N];

int tt = 0, dfn[MAX_N], low[MAX_N];
stack<int> s;
int belong[MAX_N];
bool in_stack[MAX_N];
int scc = 0;

void dfs(int u, int p)
{
    dfn[u] = low[u] = tt++;
    s.push(u);
    in_stack[u] = true;

    for (int i = 0; i < (int)g[u].size(); i++) {
        const int &v = g[u][i];

        if (dfn[v] == -1) {
            dfs(v, u);
            low[u] = min(low[u], low[v]);
        } else {
            if (in_stack[v])
                low[u] = min(low[u], dfn[v]);
        }
    }

    if (dfn[u] == low[u]) {
        scc++;
        while (1) {
            int v = s.top();
            s.pop();
            belong[v] = scc;
            in_stack[v] = false;

            if (u == v)
                break;
        }
    }
}
```

## 驗證：[POJ 2186 Popular Cows](http://poj.org/problem?id=2186)

縮 SCC 點之後，如果只有一個 DAG，那 out degree 為零的那個點就會答案，因為大家都會到達那裡！詳細解釋請看[這裏](https://amoshyc.github.io/ojsolution-build/poj/p2186.html)。

```c++
#include <algorithm>
#include <cstdio>
#include <cstring>
#include <stack>
#include <vector>

using namespace std;

const int MAX_N = 11111;
vector<int> g[MAX_N];

int tt = 0, dfn[MAX_N], low[MAX_N];
stack<int> s;
int belong[MAX_N];
bool in_stack[MAX_N];
int scc = 0;

void dfs(int u, int p)
{
    dfn[u] = low[u] = tt++;
    s.push(u);
    in_stack[u] = true;

    for (int i = 0; i < (int)g[u].size(); i++) {
        const int &v = g[u][i];

        // if(v == p) // wrong condition, sample input can reveal this error
        // continue;

        if (dfn[v] == -1) {
            dfs(v, u);
            low[u] = min(low[u], low[v]);
        } else {
            if (in_stack[v])
                low[u] = min(low[u], dfn[v]);
        }
    }

    if (dfn[u] == low[u]) {
        scc++;
        while (1) {
            int v = s.top();
            s.pop();
            belong[v] = scc;
            in_stack[v] = false;

            if (u == v)
                break;
        }
    }
}

int main()
{
    int n, m;
    while (scanf("%d %d", &n, &m) == 2) {
        for (int i = 0; i < n; i++)
            g[i].clear();

        for (int i = 0; i < m; i++) {
            int u, v;
            scanf("%d %d", &u, &v);
            u--;
            v--;

            g[u].push_back(v);
        }

        tt = 0;
        scc = 0;
        memset(dfn, -1, sizeof(dfn));
        memset(low, -1, sizeof(low));
        memset(belong, 0, sizeof(belong));
        memset(in_stack, false, sizeof(in_stack));
        for (int i = 0; i < n; i++) {
            if (dfn[i] == -1) {
                dfs(i, -1);
            }
        }

        int inDeg[scc + 1], outDeg[scc + 1];
        memset(inDeg, 0, sizeof(inDeg));
        memset(outDeg, 0, sizeof(outDeg));
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < (int)g[i].size(); j++) {
                int u = i;
                int v = g[i][j];

                if (belong[u] != belong[v]) {
                    inDeg[belong[v]]++;
                    outDeg[belong[u]]++;
                }
            }
        }

        int cnt_no_out = 0, who;
        for (int i = 1; i <= scc; i++) { // 注意 scc 區間，別被弄混了
            if (outDeg[i] == 0) {
                cnt_no_out++;
                who = i;
            }
        }

        if (cnt_no_out != 1)
            printf("0\n");
        else {
            int ans = 0;
            for (int i = 0; i < n; i++)
                if (belong[i] == who)
                    ans++;
            printf("%d\n", ans);
        }
    }

    return 0;
}

```
