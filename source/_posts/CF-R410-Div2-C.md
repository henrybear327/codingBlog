---
title: Codeforces Round 410 Div2 Problem C
date: 2017-04-27 15:26:02
categories:
  - Competitive Programming
  - Codeforces
tags:
  - Codeforces
  - Math
  - GCD
---

# [Mike and gcd problem](http://codeforces.com/contest/798/problem/C)

## Solution sketch

這個[講解](http://www.cnblogs.com/fu3638/p/6754043.html)比官方解析棒很多!

大體而言，全部都是偶數，一定ok。其餘，觀察兩奇數需要一次操作即可變成偶數偶數，一奇數一偶數需要兩次操作。

注意 `gcd(all number) != 1` 這個特判不能省下 (Wrong answer on test 40)。

<!-- more -->

## AC code

```c++
#include <bits/stdc++.h>

using namespace std;

void change(int &a, int &b)
{
    int tmp = b;
    b = a + b;
    a = a - tmp;
}

int gcd(int a, int b)
{
    return a == 0 ? b : gcd(b % a, a);
}

int main()
{
    int n;
    scanf("%d", &n);

    vector<int> inp;
    int g = 0; // default value can't be 1, come on bro
    for (int i = 0; i < n; i++) {
        int tmp;
        scanf("%d", &tmp);

        inp.push_back(tmp);
        g = gcd(g, tmp);
    }

    int cnt = 0;
    for (int i = 1; i < n; i++) {
        if ((inp[i - 1] % 2 == 1) && (inp[i] % 2 == 1)) {
            change(inp[i - 1], inp[i]);
            cnt++;
        }
    }

    for (int i = 1; i < n; i++) {
        if ((inp[i - 1] % 2 == 1) || (inp[i] % 2 == 1)) {
            change(inp[i - 1], inp[i]);
            change(inp[i - 1], inp[i]);
            cnt += 2;
        }
    }

    printf("YES\n");
    printf("%d\n", g > 1 ? 0 : cnt);

    return 0;
}

```
