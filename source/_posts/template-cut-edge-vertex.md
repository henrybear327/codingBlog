---
title: (Template) Articulation point and Bridge
categories:
  - Competitive Programming
  - Template
  - Graph
date: 2017-05-02 09:01:32
tags:
  - Template
  - Articulation point
  - Bridge
  - UVa
---

割點 與 割邊 模板

基本思維就是找一點把圖吊起來（利用DFS），並利用 back edge 來更新 low 值。

<!-- more -->

# Articulation point

## 模板

```c++
const int MAX_N = 111;
vector<int> g[MAX_N];

int tt = 0, root;
int dfn[MAX_N], low[MAX_N]; // init to -1
bool isCutVertex[MAX_N];
void dfs(int u, int p)
{
    dfn[u] = low[u] = tt++;

    int child = 0;
    for (auto v : g[u]) {
        if (v == p)
            continue;
        child++;

        if (dfn[v] == -1) {
            dfs(v, u);

            low[u] = min(low[u], low[v]);

            if (u != root && low[v] >= dfn[u]) {
                isCutVertex[u] = true;
            } else if (u == root && child >= 2) {
                isCutVertex[u] = true;
            }
        } else {
            // u -> v, u has direct access to v -> back edge
            low[u] = min(low[u], dfn[v]);
        }
    }
}
```

## 驗證：[UVa 315 Network](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=251)

```c++
#include <bits/stdc++.h>

using namespace std;

const int MAX_N = 111;
vector<int> g[MAX_N];

int tt = 0, root;
int dfn[MAX_N], low[MAX_N]; // init to -1
bool isCutVertex[MAX_N];
void dfs(int u, int p)
{
    dfn[u] = low[u] = tt++;

    int child = 0;
    for (auto v : g[u]) {
        if (v == p)
            continue;
        child++;

        if (dfn[v] == -1) {
            dfs(v, u);

            low[u] = min(low[u], low[v]);

            if (u != root && low[v] >= dfn[u]) {
                isCutVertex[u] = true;
            } else if (u == root && child >= 2) {
                isCutVertex[u] = true;
            }
        } else {
            // u -> v, u has direct access to v -> back edge
            low[u] = min(low[u], dfn[v]);
        }
    }
}

int main()
{
    char inp[10000];
    while (fgets(inp, 10000, stdin) != NULL) {
        int n;
        sscanf(inp, "%d", &n);
        if (n == 0)
            break;

        for (int i = 0; i < n; i++)
            g[i].clear();
        while (fgets(inp, 10000, stdin) != NULL) {
            char *u = strtok(inp, " ");

            if (atoi(u) == 0)
                break;
            int uu = atoi(u) - 1;

            char *v = strtok(NULL, " ");
            while (v != NULL) {
                int vv = atoi(v) - 1;
                g[uu].push_back(vv);
                g[vv].push_back(uu);
                v = strtok(NULL, " ");
            }
        }

        /*
        for(int i = 0; i < n; i++) {
                printf("u = %d: ", i + 1);
                for(auto j : g[i])
                        printf("%d ", j + 1);
                printf("\n");
        }
        */

        memset(dfn, -1, sizeof(dfn));
        memset(low, -1, sizeof(low));
        memset(isCutVertex, false, sizeof(isCutVertex));
        tt = 0;
        for (int i = 0; i < n; i++)
            if (dfn[i] == -1) {
                root = i;
                dfs(i, -1);
            }

        int ans = 0;
        for (int i = 0; i < n; i++)
            if (isCutVertex[i]) {
                // printf("%d\n", i + 1);
                ans++;
            }

        printf("%d\n", ans);
    }
    return 0;
}

```

# Bridge

## 模板

```c++
const int MAX_N = 1111;
vector<int> g[MAX_N];

int tt = 0, dfn[MAX_N], low[MAX_N];
typedef pair<int, int> ii;
vector<ii> ans;
void dfs(int u, int p)
{
    dfn[u] = low[u] = tt++;

    for (auto v : g[u]) {
        if (v == p)
            continue;

        if (dfn[v] == -1) {
            dfs(v, u);
            low[u] = min(low[u], low[v]);

            if (low[v] > dfn[u])
                ans.push_back(ii(min(u, v), max(u, v)));
        } else {
            low[u] = min(low[u], dfn[v]);
        }
    }
}
```

## 驗證：[UVa 796 Critical Links](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=737)

```c++
#include <bits/stdc++.h>

using namespace std;

const int MAX_N = 1111;
vector<int> g[MAX_N];

int tt = 0, dfn[MAX_N], low[MAX_N];
typedef pair<int, int> ii;
vector<ii> ans;
void dfs(int u, int p)
{
    dfn[u] = low[u] = tt++;

    for (auto v : g[u]) {
        if (v == p)
            continue;

        if (dfn[v] == -1) {
            dfs(v, u);
            low[u] = min(low[u], low[v]);

            if (low[v] > dfn[u])
                ans.push_back(ii(min(u, v), max(u, v)));
        } else {
            low[u] = min(low[u], dfn[v]);
        }
    }
}

int main()
{
    int n;
    while (scanf("%d", &n) == 1) {
        for (int i = 0; i < n; i++)
            g[i].clear();

        for (int i = 0; i < n; i++) {
            int u, cnt;
            scanf("%d (%d)", &u, &cnt);

            for (int j = 0; j < cnt; j++) {
                int v;
                scanf("%d", &v);

                g[u].push_back(v);
            }
        }

        memset(dfn, -1, sizeof(dfn));
        memset(low, -1, sizeof(low));
        tt = 0;
        ans.clear();
        for (int i = 0; i < n; i++) {
            if (dfn[i] == -1) {
                dfs(i, -1);
            }
        }

        sort(ans.begin(), ans.end());
        printf("%d critical links\n", (int)ans.size());
        for (auto i : ans)
            printf("%d - %d\n", i.first, i.second);
        printf("\n");
    }

    return 0;
}

```

# 綜合版本

```c++
const int MAX_N = 1111;
vector<int> g[MAX_N];

// for bridge
typedef pair<int, int> ii;
vector<ii> ans;

// for articulation point
int root; // set it before dfs() call
bool isCutVertex[MAX_N]; // init to false

int tt = 0, dfn[MAX_N], low[MAX_N]; // init array to -1
void dfs(int u, int p)
{
    dfn[u] = low[u] = tt++;

    // for articulation point, root needs to have >= 2 childrens
    int child = 0;
    for (auto v : g[u]) {
        if (v == p)
            continue;
        child++;

        if (dfn[v] == -1) {
            dfs(v, u);
            low[u] = min(low[u], low[v]);

            if (low[v] > dfn[u]) // bridge
                ans.push_back(ii(min(u, v), max(u, v)));

            if (u != root && low[v] >= dfn[u]) { // articulation point
                isCutVertex[u] = true;
            } else if (u == root && child >= 2) { // articulation point
                isCutVertex[u] = true;
            }
        } else {
            // u -> v, u has direct access to v -> back edge
            low[u] = min(low[u], dfn[v]);
        }
    }
}

```
