---
title: ITSA 2016 桂冠賽 闖關組
categories:
  - Competitive Programming
  - ITSA 桂冠賽
  - 2016
date: 2017-05-01 11:19:33
tags:
  - ITSA 桂冠賽
---

# ITSA 2016 桂冠賽 闖關組

只寫了第一到第六題，希望學弟妹們看得懂哈哈。

<!-- more -->

# 1

開 long long 就對了

```c++
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <algorithm>
#include <vector>

using namespace std;

typedef long long ll;

int main()
{
    int ncase;
    scanf("%d", &ncase);

    while (ncase--) {
        ll n, Q;
        scanf("%lld %lld", &n, &Q);

        ll product = 1;
        int ok = -1;
        ll inp[n];
        for (int i = 0; i < n; i++)
            scanf("%lld", &inp[i]);
        for (int i = 0; i < n; i++) {
            product = ((product % Q) * (inp[i] % Q)) % Q;
            if (product == 1) {
                ok = i + 1;
                break;
            }
        }

        printf("%d\n", ok);
    }

    return 0;
}
```

# 2

直接模擬即可

```c++
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <algorithm>
#include <vector>

using namespace std;

typedef long long ll;

int main()
{
    int ncase;
    scanf("%d", &ncase);

    while (ncase--) {
        int n;
        scanf("%d", &n);

        int cards[53];
        for (int i = 1; i <= 52; i++)
            cards[i] = i;

        for (int i = 0; i < n; i++) {
            int num;
            scanf("%d", &num);

            int tmp[53];
            for (int j = num; j <= 52; j++) {
                tmp[j - num + 1] = cards[j];
            }
            for (int j = 1; j < num; j++) {
                tmp[j + (52 - num + 1)] = cards[j];
            }
            memcpy(cards, tmp, sizeof(tmp));
        }
        printf("%d\n", cards[52]);
    }

    return 0;
}
```

# 3

吃RE的...
Array開大一點就對啦QQQQ
或是用vector...

```c++
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <algorithm>
#include <vector>

using namespace std;

typedef long long ll;

int num[1000001];

int main()
{
    int cnt = 1;
    while (scanf("%d", &num[cnt]) == 1)
        cnt++;
    cnt--;

    sort(num + 1, num + cnt + 1);
    printf("%d\n", cnt % 2 == 0 ? num[cnt / 2] : num[(cnt + 1) / 2]);
}
```

```c++
#include <algorithm>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <vector>

using namespace std;

typedef long long ll;

int main()
{
    vector<int> inp;
    int num;
    while (scanf("%d", &num) == 1)
        inp.push_back(num);

    sort(inp.begin(), inp.end());
    int cnt = (int)inp.size();
    printf("%d\n", cnt % 2 == 0 ? inp[cnt / 2 - 1] : inp[(cnt + 1) / 2 - 1]);
}
```

# 4

不用推公式
直接for迴圈

```c++
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <algorithm>
#include <vector>

using namespace std;

typedef long long ll;

int main()
{
    int ncase;
    scanf("%d", &ncase);

    while (ncase--) {
        int n;
        scanf("%d", &n);
        int sum = 0, ans = 0;
        for (int i = 0; i < n; i++) {
            int w;
            scanf("%d", &w);

            ans += (sum + (w / 2));
            sum += w;
        }

        printf("%d\n", ans);
    }
}
```

# 5

對於每個點，枚舉出他的相鄰兩點後，檢查他們之間的連通性即可。

```c++
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <algorithm>
#include <vector>

using namespace std;

typedef long long ll;

int main()
{
    int ncase;
    scanf("%d", &ncase);

    while (ncase--) {
        int n;
        scanf("%d", &n);

        int inp[n][n];
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                scanf("%d", &inp[i][j]);

        int ans = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                for (int k = j + 1; k < n; k++) {
                    if (inp[i][j] == 1 && inp[i][k] == 1 && inp[j][k] == 1)
                        ans++;
                }
            }
        }

        printf("%d\n", ans);
    }
}
```

# 6

先建出可能可以湊出的數字表!

```c++
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <algorithm>
#include <vector>

using namespace std;

typedef long long ll;

int main()
{
    int ncase;
    scanf("%d", &ncase);

    while (ncase--) {
        int n, m;
        scanf("%d %d", &n, &m);

        bool possible[200010] = {0};
        for (int i = 0; i < n; i++) {
            int num;
            scanf("%d", &num);

            for (int j = 200010 - 1; j >= 0; j--) {
                if (j - num >= 0 && possible[j - num] == true)
                    possible[j] = true;
            }
            possible[num] = true;
        }

        int ans = 0;
        for (int i = 0; i < m; i++) {
            int num;
            scanf("%d", &num);

            if (possible[num] == true)
                ans++;
        }
        printf("%d\n", ans);
    }
}
```
