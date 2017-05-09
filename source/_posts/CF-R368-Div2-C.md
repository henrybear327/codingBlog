---
title: Codeforces Round 368 Div2 Problem C
categories:
  - Competitive Programming
  - Codeforces
tags:
  - Codeforces
  - Math
date: 2016-08-21 08:42:59
---


# [CF707C Pythagorean Triples](http://codeforces.com/contest/707/problem/C)

## Solution Sketch

Using `Euclid's formula`

{% math %}
\begin{align*}
   a& = p^2 - q^2  \tag 1\\
   b& = 2pq  \tag 2\\
   c& = p^2 + q^2 \tag 3\\
\end{align*}
{% endmath %}

<!-- more -->

There are two special cases, $n = 1$ and $n = 2$, where the right triangle can't be formed (plug in the numbers and you will see why).

After eliminating special cases, we just need to deal with $n$ according to parity.

If $n$ is **odd**, we can use $\text{equation $(1)$}$ and property
$$
\begin{align}
(k + 1)^2 - k^2 &= 2k + 1 &&\text{the difference between two consecutive numbers' square is odd}\tag 4
\end{align}
$$
We can get $k = \frac{n - 1}{2}$ (because $2k + 1 = n$), and then substitute $k$ with $\frac{n - 1}{2}$ in $\text{equation $(4)$}$ to obtain $q = \frac{n - 1}{2} + 1$ and $p = \frac{n - 1}{2}$. We can get the answers for odd from $\text{equation $(2)$ and equation $(3)$}$, which are $$2 (\frac{n - 1}{2} + 1) (\frac{n - 1}{2}) \;\text{and}\; (\frac{n - 1}{2} + 1)^2 + (\frac{n - 1}{2})^2$$

If $n$ is **even**, we can use $\text{equation $(2)$}$ and let $q = 1$ (then $p = \frac n2$). We can the get the answers for even from $\text{equation $(1)$ and equation $(3)$}$, which are $$(\frac{n}{2})^2 - 1 \;\text{and}\; (\frac{n}{2})^2 + 1$$

## AC Code

```c++
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

int main()
{
	ll n;
	scanf("%lld", &n);

	if(n <= 2)
		printf("-1\n");
	else {
		if(n & 1) {
			n = (n - 1) / 2;
			printf("%lld %lld\n", 2 * (n + 1) * n, (n + 1) * (n + 1) + n * n);
		} else {
			n /= 2;
			printf("%lld %lld\n", n * n - 1, n * n + 1);
		}
	}

	return 0;
}
```
