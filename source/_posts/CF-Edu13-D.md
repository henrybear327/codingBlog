---
title: Codeforces Educational Round 13 Problem D
categories:
  - Competitive Programming
  - Codeforces
tags:
  - Codeforces
  - Fast exponentiation
  - Matrix
date: 2016-08-20 01:37:16
---


# [CFEdu13D Iterated Linear Function](http://codeforces.com/contest/678/problem/D)

## Solution Sketch

Observe the following formula, you can see that the answer is hidden in it!

{% math %}
\begin{aligned}
        \begin{bmatrix}
        a & b \\
        0 & 1 \\
        \end{bmatrix}^{n}
        \begin{bmatrix}
        x \\
        1 \\
        \end{bmatrix}
        =
        \begin{bmatrix}
        f_n\\
        1 \\
        \end{bmatrix}
\end{aligned}
{% endmath %}

<!-- more -->

---

Expansion:

$n = 1$
{% math %}
\begin{aligned}
        \begin{bmatrix}
        a & b \\
        0 & 1 \\
        \end{bmatrix}
        \begin{bmatrix}
        x \\
        1 \\
        \end{bmatrix}
        =
        \begin{bmatrix}
        ax + b\\
        1 \\
        \end{bmatrix}
\end{aligned}
{% endmath %}

$n = 2$, uses result from $n = 1$
{% math %}
\begin{aligned}
        \begin{bmatrix}
        a & b \\
        0 & 1 \\
        \end{bmatrix}^{2}
        \begin{bmatrix}
        x \\
        1 \\
        \end{bmatrix}
        =
        \begin{bmatrix}
        a & b \\
        0 & 1 \\
        \end{bmatrix}
        \begin{bmatrix}
        ax + b\\
        1 \\
        \end{bmatrix}
        =
        \begin{bmatrix}
        a(ax + b) + b\\
        1 \\
        \end{bmatrix}
\end{aligned}
{% endmath %}

## AC Code (Using operator overloading)

```c++
#include <bits/stdc++.h>

using namespace std;
typedef long long int ll;

const ll M = ((ll)1e9 + 7);

ll a, b, x, n;

struct matrix {
    ll data[2][2];

    matrix()   // default constructor
    {
        for (int i = 0; i < 2; i++)
            for (int j = 0; j < 2; j++)
                data[i][j] = 0;
    }

    matrix operator*(matrix q)   // overload *
    {
        matrix ans;
        for (int i = 0; i < 2; i++)
            for (int j = 0; j < 2; j++)
                for (int k = 0; k < 2; k++)
                    ans.data[i][j] =
                        (ans.data[i][j] + (data[i][k] * q.data[k][j]) % M) % M;
        return ans;
    }
};

ll fast_pow(ll exp)
{
    matrix base;
    base.data[0][0] = a;
    base.data[0][1] = b;
    base.data[1][1] = 1;

    // res = base ^ n
    matrix res;
    res.data[0][0] = res.data[1][1] = 1;
    while (exp) {
        if (exp & 1) {
            res = res * base;
        }
        base = base * base;
        exp >>= 1;
    };

    // res1 = res * [x, 1]
    ll res1 = (res.data[0][0] * x % M + res.data[0][1]) % M;
    return res1 % M;
}

int main()
{
    scanf("%lld %lld %lld %lld", &a, &b, &n, &x);

    // fast pow
    printf("%lld\n", fast_pow(n));

    return 0;
}

```

## AC code

```c++
#include <bits/stdc++.h>

using namespace std;
typedef long long int ll;

const ll M = ((ll)1e9 + 7);

ll a, b, x, n;

void mul(ll p[2][2], ll q[2][2])
{
    ll tmp[2][2];
    memset(tmp, 0, sizeof(tmp));
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 2; j++) {
            for (int k = 0; k < 2; k++) {
                tmp[i][j] = (tmp[i][j] + (p[i][k] * q[k][j]) % M) % M;
            }
        }
    }

    memcpy(p, tmp, sizeof(tmp));
}

ll fast_pow(ll exp)
{
    ll base[2][2] = {{a, b}, {0, 1}};

    // res = base ^ n
    ll res[2][2] = {{1, 0}, {0, 1}};
    while (exp) {
        if (exp & 1) {
            mul(res, base);
        }
        mul(base, base);
        exp >>= 1;
    };

    // res1 = res * [x, 1]
    ll res1 = (res[0][0] * x % M + res[0][1]) % M;
    return res1 % M;
}

int main()
{
    scanf("%lld %lld %lld %lld", &a, &b, &n, &x);

    // fast pow
    printf("%lld\n", fast_pow(n));

    return 0;
}

```
