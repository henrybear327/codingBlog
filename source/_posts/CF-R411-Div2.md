---
title: Codeforces Round 411 Division 2
categories:
  - Competitive Programming
  - Codeforces
  - Division 2
date: 2017-05-07 10:57:09
tags:
  - Codeforces
  - Math
  - String
---

# [Codeforces Round 411 Division 2](http://codeforces.com/contest/805)

A 是不錯數學的觀察題

B 是不錯的回文有趣小題

D 是不錯的規律觀察題

<!-- more -->

## A

### Solution sketch

如果只有一個數字的區間，輸出本身。

其餘，都可以輸出二。重要的觀察是偶數一定會相隔相隔兩數就出現一次，奇數的話最少相隔 3 個數字才會重複一個相同的 divisor。

### AC code

```c++
#include <bits/stdc++.h>

using namespace std;

int main()
{
	int l, r;
	scanf("%d %d", &l, &r);
	if(l == r)
		printf("%d\n", l);
	else
		printf("2\n");

	return 0;
}

```

## B

### Solution sketch

`aabb....`

### AC code

```c++
#include <bits/stdc++.h>

using namespace std;

int main()
{
	int n;
	scanf("%d", &n);

	const char* str = "aabb";
	for(int i = 0; i < n; i++)
		printf("%c", str[i % 4]);

	return 0;
}

```

## D

### Solution sketch

推規律。

有個重要小觀察是說 最終狀態一定是 `bbbbb....aaaa.....` 的型態。

所以，從左往右一個一個看，你會發現遇到 b 時，他的左邊所有 a 都會被搬到他的右邊去，而且搬移次數會是 $2^{a 目前累計個數} - 1$。

因此，每看到一個 b，我們可以利用他左邊的 a 個數 來算出搬移次數。那從左到右看一遍，也就可以得到答案了。

### AC code

```c++
#include <bits/stdc++.h>

using namespace std;

int main(){
	char inp[1111111];
	scanf("%s", inp);

	int len = strlen(inp);
	long long mod = (1e9) + 7, cnt = 0, base = 1;
	for(int i = 0; i < len; i++) {
		if(inp[i] == 'a') {
			base *= 2;
			base %= mod;
		} else {
			cnt += base;
			cnt--;
			cnt += mod;
			cnt %= mod;
		}
	}
	printf("%lld\n", cnt);
}

```
