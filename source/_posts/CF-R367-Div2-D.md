---
title: Codeforces Round 367 Div2 Problem D
categories:
  - Competitive Programming
  - Codeforces
tags:
  - Codeforces
  - Trie
date: 2016-08-12 14:31:21
---


# [CF706D Vasiliy's Multiset](http://codeforces.com/contest/706/problem/D)

## Solution Sketch

We know that $ 1 \oplus 0 = 0 \oplus 1 = 1$ and $ 0 \oplus 0 = 1 \oplus 1 = 0$ ($\oplus$ stands for XOR). Thus, in order to find the maximum XOR value of two numbers, we need to choose a number such that it has as many opposite bits as possible regarding $x$.

How can we do it efficiently? Binary trie is here to save us!

<!-- more -->

We can view every number in their binary representation form.
1. If the query asked us to insert a number, we simply add the number bit by bit: if the current bit is 0, we move to the left subtree. Otherwise, move to the right one. **Notice:** We will pad zeros in front of some numbers in order to make every number of the same length. This will make querying and removing easy.
2. If the query asked for the maximum XOR value regarding a specific $x$, simply traverse the binary trie (look for the solution greedily). If the current bit of $x$ is 1, then see if the trie has a left subtree (0). If yes, then move to that subtree. Otherwise, move to the other one (1).
```
       root
      /    \
   0 /      \ 1
    /        \
  left     right
  subtree  subtree
```

# AC Code

```c++
#include <bits/stdc++.h>

using namespace std;

typedef long long int ll;
typedef pair<int, int> ii;

const int len = 30; // 1 << 30 -> 31 bits

struct Data {
    int lc, rc;
    int val;
};

#define DEPTH 30
#define N 200100 * DEPTH

struct Trie {
    // 0 is root
    Data data[N];
    int nextAvail;

    void init()
    {
        memset(data, -1, sizeof(data));
        nextAvail = 1;
    }

    void insert(int x)
    {
        insert(x, DEPTH, 0);
    }

    void insert(int x, int shift, int node)
    {
        // printf("%d %d %d\n", x, shift, node);
        if (shift == -1) {
            data[node].val = x;

            return;
        }

        if (x & (1 << shift)) { // 1 rc
            if (data[node].rc == -1) {
                data[node].rc = nextAvail++;
            }

            insert(x, shift - 1, data[node].rc);
        } else { // 0 lc
            if (data[node].lc == -1) {
                data[node].lc = nextAvail++;
            }

            insert(x, shift - 1, data[node].lc);
        }
    }

    void remove(int x)
    {
        bool var = true;
        remove(x, DEPTH, 0, &var);
    }

    void remove(int x, int shift, int node, bool *del)
    {
        if (shift == -1) {
            return;
        }

        int side = (x & (1 << shift)) ? data[node].rc : data[node].lc;
        remove(x, shift - 1, side, del);

        if (*del == true) {
            if (x & (1 << shift))
                data[node].rc = -1;
            else
                data[node].lc = -1;

            if (data[node].lc != -1 || data[node].rc != -1)
                *del = false;
        }
    }

    int query(int x)
    {
        return query(x, DEPTH, 0);
    }

    int query(int x, int shift, int node)
    {
        if (shift == -1) {
            return data[node].val;
        }

        int tmp;
        if (x & (1 << shift)) {
            if (data[node].lc != -1)
                tmp = data[node].lc;
            else
                tmp = data[node].rc;
        } else {
            if (data[node].rc != -1)
                tmp = data[node].rc;
            else
                tmp = data[node].lc;
        }
        return query(x, shift - 1, tmp);
    }
} trie;

int main()
{
    int n;
    scanf("%d", &n);

    trie.init();

    trie.insert(0);
    map<int, int> cnt;
    for (int i = 0; i < n; i++) {
        char inp[1000];
        int x;
        scanf("%s %d", inp, &x);

        if (inp[0] == '+') {
            cnt[x]++;
            if (cnt[x] == 1)
                trie.insert(x);
        } else if (inp[0] == '-') {
            cnt[x]--;
            if (cnt[x] == 0) {
                trie.remove(x);
                cnt.erase(x);
            }
        } else {
            printf("%d\n", x ^ trie.query(x));
        }
    }

    return 0;
}

```
