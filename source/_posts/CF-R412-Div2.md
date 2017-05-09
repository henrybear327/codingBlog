---
title: Codeforces Round 412 Division 2
categories:
  - Competitive Programming
  - Codeforces
  - Division 2
date: 2017-05-08 09:19:42
tags:
  - Codeforces
  - Implementation
---

# [Codeforces Round 412 Division 2](http://codeforces.com/contest/807)

tourist 出題呀！ WOW!

A 是不錯思考題

<!-- more -->

## A

### Solution sketch

其實按照題目走就可以了。

如果有人的 rating 有更動，那一定是 rated。

如果大家 rating 都沒變動的話，就得靠題目的 `It's also known that if the round was rated and a participant with lower rating took a better place in the standings than a participant with higher rating, then at least one round participant's rating has changed.`來判斷。如果 低rating的人 排名比 高rating的人前面，但是比完賽後 rating 沒變，那就一定是 unrated。但是，如果已經呈現非嚴格遞減排列，我們就無從得知了！因為比完賽後排名一樣 rating 有可能持平，也可能根本就 unrated 導致 rating 沒變，所以答案就 maybe 啦。

### AC code

```c++
#include <bits/stdc++.h>

using namespace std;

typedef pair<int, int> ii;
int main()
{
    int n;
    scanf("%d", &n);

    int state = 0; // unrated
    vector<ii> inp;
    for (int i = 0; i < n; i++) {
        int a, b;
        scanf("%d %d", &a, &b);

        inp.push_back(ii(a, b));

        if (a != b)
            state = 1; // rated
    }

    bool sorted = true;
    for (int i = 1; i < n; i++) {
        if (inp[i] > inp[i - 1])
            sorted = false;
    }

    if (state == 0 and sorted)
        state = 2;

    if (state == 0)
        printf("unrated\n");
    else if (state == 1)
        printf("rated\n");
    else
        printf("maybe\n");

    return 0;
}

```
