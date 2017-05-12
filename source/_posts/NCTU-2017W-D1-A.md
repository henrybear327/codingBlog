---
title: UVA11311
date: 2017-01-16 16:07:22
categories:
  - Competitive Programming
  - UVa
tags:
  - Game theory
  - Sprague Grundy Number
  - UVa
---

# [Exclusively Edible](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=2286)

## AC Code

```c++
#include <bits/stdc++.h>

using namespace std;

int main()
{
	int ncase;
	scanf("%d", &ncase);

	while(ncase--) {
		int r, c, x, y;
		scanf("%d %d %d %d", &r, &c, &x, &y);

		int val = (x + 1) ^ (r - x) ^ (y + 1) ^ (c - y);
		printf("%s\n", val == 0 ? "Hansel" : "Gretel");
	}

	return 0;
}
```
