---
layout: page
title: "source code of poj2456"
date: 2015-03-14 19:36
comments: true
sharing: true
footer: true
---

```c++
#include <iostream>
#include <cstdio>
#include <cstring>
#include <string>
#include <set>
#include <algorithm>
#include <map>
#include <queue>
#include <vector>
#include <cmath>

using namespace std;
const int INF = 0x3f3f3f3f;
const int maxn = 111111;
typedef long long ll;
ll x[maxn];
int N,C;
ll maxx;
const double eps = 1e-4;
int rec;
bool canMake(int dis)
{
    int cnt = 1;
    rec = x[0];
    for (int i = 1; i < N; i++)
    {
        if(x[i] - rec >= dis)
        {
            cnt++;
            rec = x[i];
        }

        if(cnt >= C)
            return true;
    }

    return false;
}

void solve()
{
    sort(x, x+N);
    ll l = 0, r = x[N-1];
    while(r-l > 1)
    {
        int mid = (l+r)>>1;
        if (canMake(mid))
            l = mid;
        else
            r = mid;
    }
    printf("%I64d\n", l);
}

int main()
{
    while(scanf("%d%d", &N, &C) == 2)
    {
        for (int i = 0; i < N; i++)
            scanf("%I64d", &x[i]);
        solve();
    }
    return 0;
}
```

[<-Back](/blog/2015/02/13/er-fen-cha-zhao-ji-qi-ying-yong/)