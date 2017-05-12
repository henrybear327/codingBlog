---
title: ITSA 2016 桂冠賽 挑戰組 第五題
categories:
  - Competitive Programming
  - ITSA 桂冠賽
  - 2016
date: 2017-05-03 21:10:21
tags:
  - ITSA 桂冠賽
  - Implementation
  - Math
---

# [Quotient Harmonic Series 商調和級數](http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?a=15725)

其實我覺得這題有個觀察是說，$m$ 所除上的除數可以看成 $index$，然後 $\frac{m}{除數} = 商$ 且 $\frac{m}{商} = 除數$。

這題有點得從操作中感覺一下，所以拿 $m = 9$ 來示範一下吧。

我們倒著找回來，先設定除數為 $b = 9$，$q = \frac{m}{b} = 1$，這個商也就是要拿來加總的數值。個數的話，利用文章一開始所說的觀察，我們可以知道 $b\prime = \frac{m}{(q + 1)} = 4$ 就會是下一個非此 $q$ 的位置，也就是除數。所以 $b\prime - b$ 就是重複次數，且下一回合的 $b$ 就是 $b\prime$。

<!-- more -->

## AC code

```c++
#include <cstdio>

typedef long long ll;
int main()
{
    int ncase;
    scanf("%d", &ncase);

    while (ncase--) {
        ll m;
        scanf("%lld", &m);

        ll b = m;
        ll ans = 0;
        while (b > 0) {
            ll q = m / b;
            ll b1 = m / (q + 1);
            ans += q * (b - b1);
            b = b1;
        }
        printf("%lld\n", ans);
    }

    return 0;
}
```
