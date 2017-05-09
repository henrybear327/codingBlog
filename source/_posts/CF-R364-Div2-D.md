---
title: Codeforces Round 364 Div2 Problem D
date: 2016-08-04 15:27:38
categories:
  - Competitive Programming
  - Codeforces
tags:
  - Codeforces
  - Math
---

# [CF701D As Fast As Possible](http://codeforces.com/contest/701/problem/D)

## Solution Sketch

One crucial observation is: all pupils must spend the same amount of time on bus, or the last student to arrive will need a longer time than the optimal solution.

<!-- more -->

* p = groups to be taken by bus = `(n - 1) / k + 1`
* t1 = time on foot
* t2 = time on bus
* t3 = time between the bus drop off one group of students and pick up anther group of students

The diagram for the first two groups can be illustrated as followed:
```
--------> t2 * v2 (group 1 on the bus)
      <-- t3 * v2 (drop off group 1, and then the bus returned)
      --------> t2 * v2 (group 2 picked up by the bus)
-----> (t2 + t3) * v1 (group 2 time on foot)
```

From the above diagram, one can get 3 equations
```
t2 * v2 * p - t3 * v2 * (p - 1) = l
t1 * v1 + t2 * v2 = l
(t2 + t3) * v1 + t3 * v2 = t2 * v2
```

Solve the equations, you can get `t1`, `t2`, `t3`, where `t1 + t2` is the answer.

## AC Code

```c++
#include <bits/stdc++.h>

using namespace std;

typedef long long int ll;
typedef pair<int, int> ii;

#define EPS 1e-9
int main()
{
    ll n, l, v1, v2, k;
    scanf("%lld %lld %lld %lld %lld", &n, &l, &v1, &v2, &k);

	// ll p = n / k + (n % k == 0 ? 0 : 1);
	ll p = (n - 1) / k + 1;
	double t3 = (double)((v2 - v1) * l) / (double)(v2 * p * (v1 + v2) + (v1 - v2) * v2 * (p - 1));
	double t2 = (double)(l + t3 * v2 * (p - 1)) / (v2 * p);
	double t1 = (double)(l - t2 * v2) / v1;

	printf("%.15f\n", t1 + t2);

    return 0;
}
```
