---
title: N 皇后，回味大一程式作業
categories:
  - Random code
date: 2017-05-01 19:14:41
tags:
  - Random code
---

# N 皇后

純粹懷舊。

<!-- more -->

## AC code

```c++
#include <bits/stdc++.h>

using namespace std;

#define N 8

bool seen[N][N];
bool row[N];

int ans = 0;
void dfs(int depth)
{
    if (depth == N) {
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                printf("%d%c", seen[i][j], j == N - 1 ? '\n' : ' ');
            }
        }
        printf("\n");
        ans++;
        return;
    }

    for (int i = 0; i < N; i++) {
        bool error = false;
        if (row[i] == 0) {
            for (int j = 1; j <= depth; j++) {
                if (i - j >= 0 && seen[depth - j][i - j] == true) {
                    error = true;
                    break;
                }
                if (i + j < N && seen[depth - j][i + j] == true) {
                    error = true;
                    break;
                }
            }

            if (error)
                continue;
            seen[depth][i] = true;
            row[i] = true;
            dfs(depth + 1);
            seen[depth][i] = false;
            row[i] = false;
        }
    }
}

int main()
{
    memset(seen, false, sizeof(seen));
    memset(row, false, sizeof(row));

    dfs(0);

    printf("total %d\n", ans);
    return 0;
}

```
