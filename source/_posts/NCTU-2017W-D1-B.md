---
title: Codeforces Round 334 Div2 Problem E
date: 2017-01-16 21:41:14
categories:
  - Competitive Programming
  - Codeforces
tags:
  - Game theory
  - Sprague Grundy Number
  - Codeforces
---

# [CF Lieges of Legendre](http://codeforces.com/contest/604/problem/E)

## Solution sketch

When $k\ is\ odd$, $[0, 20]$ = {$0, 1, 0, 1, 2, 0, 2, 0, 1, 0, 1, 0, 1, 0, 1, 0, 2, 0, 1, 0, 2$}.
So for $number <= 4$, the $sg\ value$ needs to be handled differently. For all other numbers, if it is odd, $sg\ value = 0$, otherwise, $sg\ value = (sg\ value\ of (\frac{number}{2}) == 1 ? 2 : 1)$.

When $k\ is\ even$, $[0, 8]$ = {$0, 1, 2, 0, 1, 0, 1, 0, 1...$}. So for $number < 3$, the $sg\ value$ needs to be handled differently. For all other numbers, $number \% 2 == 1 ? 0 : 1$.

<!-- more -->

## AC Code

```c++
#include <bits/stdc++.h>

using namespace std;

int n, k;
int cal(int num)
{
    if (k % 2 == 0) {
        if (num < 3)
            return num;
        return num % 2 == 1 ? 0 : 1;
    } else {
        if (num <= 4)
            return num == 4 ? 2 : num % 2;
        if (num % 2 == 1)
            return 0;
        return cal(num / 2) == 1 ? 2 : 1;
    }
}

int main()
{
    scanf("%d %d", &n, &k);

    int val = 0;
    for (int i = 0; i < n; i++) {
        int num;
        scanf("%d", &num);

        val ^= cal(num);
    }

    if (val == 0)
        printf("Nicky\n");
    else
        printf("Kevin\n");

    return 0;
}

```
