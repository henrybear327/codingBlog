---
title: Codeforces Round 365 Div2 Problem C
date: 2016-08-05 09:46:59
categories:
  - Competitive Programming
  - Codeforces
tags:
  - Codeforces
  - Geometry
---

# [CF703C Chris and Road](http://codeforces.com/contest/703/problem/C)

# Solution Sketch

## Key idea

Think of the slope of the line $m = \frac{u}{v}$. What can you do with it?

<!-- more -->

## Observation

If you draw the polygon out on the Cartesian coordinate plane and draw a line $y = mx$, you will figure out that if the line never intersects with the polygon, you won't be hit by the bus!

Otherwise, find the rightmost line using the same slope ($m = \frac{u}{v}$) by iterating over all vertices, and use the value $b$ from the rightmost line ($y = mx + b$) to calculate the minimal time to walk to the target, which is $ \frac{(w - b)}{u} $.

# AC Code

```c++
#include <bits/stdc++.h>

using namespace std;

typedef pair<int, int> ii;

#define EPS 1e-9

int main()
{
    int n, w, v, u;
    scanf("%d %d %d %d", &n, &w, &v, &u);

    vector<ii> inp;

    double m = (double)u / v;
    for (int i = 0; i < n; i++) {
        int x, y;
        scanf("%d %d", &x, &y);
        inp.push_back(ii(x, y));
    }

    int wrongSide = 0;
    for (int i = 0; i < n; i++) {
        if (m * inp[i].first - inp[i].second > -EPS)
            continue;
        else
            wrongSide++;
    }

    if (wrongSide == 0 || wrongSide == n) {
        printf("%.15f\n", (double)w / u);
    } else {
        double b = INT_MAX;

        for (int i = 0; i < n; i++) {
            b = min(b, inp[i].second - inp[i].first * m);
        }

        printf("%.15f\n", fabs((w - b) / u));
    }

    return 0;
}
```
