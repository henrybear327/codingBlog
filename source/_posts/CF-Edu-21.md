---
title: Codeforces Educational Round 21
categories:
  - Competitive Programming
  - Codeforces
date: 2017-05-19 15:25:00
tags:
  - Codeforces
  - Implementation
  - Prefix sum
  - Binary search
  - Fenwick tree
---

# [Codeforces Educational Round 21](http://codeforces.com/contest/808)

A 的觀察挺直觀的

B 應該算是經典的 區間和 題目

C 就一般先排序後操作的題目

D 是個有趣的數列題

<!-- more -->

## A 

### Solution sketch

我直接 從小到大 ， 建立出 所有可能得解 的表。對於每個 input，就從表中做操作即可。

### AC code

```c++
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

int main()
{
    int n;
    scanf("%d", &n);

    vector<ll> cand;
    for (int i = 1; i <= 9; i++)
        cand.push_back(i);

    ll base = 10;
    for (int i = 0; i < 9; i++) {
        for (int j = 1; j <= 9; j++)
            cand.push_back(j * base);

        base *= 10;
    }

    for (auto i : cand) {
        if (i - n > 0) {
            printf("%lld\n", i - n);
            break;
        }
    }

    return 0;
}

```

## B

### Solution sketch

從左到右的滑動區間，並維護區間和。

### AC code

```c++
#include <bits/stdc++.h>

using namespace std;

int main()
{
    int n, k;
    scanf("%d %d", &n, &k);

    int inp[n];
    for (int i = 0; i < n; i++)
        scanf("%d", &inp[i]);

    long long sum = 0;
    for (int i = 0; i < k; i++)
        sum += inp[i];

    long long ans = sum;
    for (int i = k; i < n; i++) {
        sum += inp[i];
        sum -= inp[i - k];

        ans += sum;
    }

    printf("%.15f\n", 1.0 * ans / (n - k + 1));

    return 0;
}

```

## C

### Solution sketch

排序後，每杯都先給一半的量。如果還有多，就從最大的杯子一路裝滿裝到沒剩餘量為止。

### AC code

```c++
#include <bits/stdc++.h>

using namespace std;

typedef pair<int, int> ii;
int main()
{
    int n, w;
    scanf("%d %d", &n, &w);

    ii cup[n];
    for (int i = 0; i < n; i++) {
        int tmp;
        scanf("%d", &tmp);

        cup[i] = ii(tmp, i);
    }

    sort(cup, cup + n);

    int fill[n];
    for (int i = 0; i < n; i++) {
        fill[cup[i].second] = (cup[i].first + 1) / 2;
        w -= fill[cup[i].second];
    }

    if (w < 0) {
        printf("-1\n");
        return 0;
    } else if (w > 0) {
        for (int i = n - 1; i >= 0; i--) {
            int remain = cup[i].first - fill[cup[i].second];

            if (w >= remain) {
                w -= remain;
                fill[cup[i].second] = cup[i].first;
            } else {
                fill[cup[i].second] += w;
                w = 0;
            }
        }

        if (w > 0) {
            printf("-1\n");
            return 0;
        }
    }

    for (int i = 0; i < n; i++)
        printf("%d%c", fill[i], i == n - 1 ? '\n' : ' ');

    return 0;
}

```

## D

### Solution sketch

待補

### AC code

利用 prefix 和 suffix 總合直接比較的版本

```c++
#include <bits/stdc++.h>

using namespace std;

const int MAX_N = 100100;

typedef long long ll;

ll inp[MAX_N];
int n;
ll sum = 0;

bool ok;
void go()
{
    ll prefixSum = 0;

    set<ll> in;
    for (int i = 0; i < n; i++) {
        prefixSum += inp[i];

        in.insert(inp[i]);
        if (prefixSum < sum / 2)
            continue;

        ll diff = (prefixSum - sum / 2);

        if (in.find(diff) != in.end()) {
            ok = true;
            return;
        }
    }
}

int main()
{
    scanf("%d", &n);

    sum = 0;
    ok = false;
    for (int i = 0; i < n; i++) {
        scanf("%lld", &inp[i]);

        sum += inp[i];
    }

    if (sum % 2 == 1) {
        ok = false;
    } else {
        go();
        reverse(inp, inp + n);
        go();
    }

    if (ok)
        printf("YES\n");
    else
        printf("NO\n");

    return 0;
}

```

利用 fenwick tree 和 binary search 的版本

```c++
#include <bits/stdc++.h>

using namespace std;

const int MAX_N = 100100;

#define LSB(x) (x & (-x))
typedef long long ll;
struct Fenwick {
    ll data[MAX_N];
    int n;

    void init(int n)
    {
        this->n = n;
        memset(data, 0, sizeof(ll) * (n + 1));
    }

    ll query(int idx)
    {
        idx++;
        ll ret = 0;
        while (idx > 0) {
            ret += data[idx];
            idx -= LSB(idx);
        }
        return ret;
    }

    void add(int idx, ll val)
    {
        idx++;
        while (idx <= n) {
            data[idx] += val;
            idx += LSB(idx);
        }
    }
};

int main()
{
    int n;
    scanf("%d", &n);

    Fenwick fw;
    fw.init(n);

    ll inp[n];
    for (int i = 0; i < n; i++) {
        scanf("%lld", &inp[i]);

        fw.add(i, inp[i]);
    }

    ll half = fw.query(n - 1) / 2;
    if ((fw.query(n - 1) % 2 == 1) || (n == 1)) {
        printf("NO\n");
        return 0;
    }

    for (int i = 0; i < n; i++) {
        fw.add(i, -inp[i]);

        {
            int l = -1, r = n;
            while (r - l > 1) {
                int mid = (l + r) / 2;

                if (fw.query(mid) <= half - inp[i]) {
                    l = mid;
                } else {
                    r = mid;
                }
            }

            ll tmp = fw.query(l);
            if (l != -1 && tmp + inp[i] == half) {
                printf("YES\n");
                return 0;
            }
        }

        {
            int l = -1, r = n;
            while (r - l > 1) {
                int mid = (l + r) / 2;

                if (fw.query(mid) <= half) {
                    l = mid;
                } else {
                    r = mid;
                }
            }

            ll tmp = fw.query(l);
            if (l != -1 && tmp == half) {
                printf("YES\n");
                return 0;
            }
        }

        fw.add(i, inp[i]);
    }

    printf("NO\n");

    return 0;
}

```
