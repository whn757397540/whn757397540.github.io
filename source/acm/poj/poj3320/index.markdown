---
layout: page
title: "Source code of poj3320"
date: 2015-02-14 19:15
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
pair<int, int> P[maxn];
int p[maxn];
int a[maxn];
int cnt[maxn];
int main()
{
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i++)
    {
        scanf("%d", &P[i].first);
        P[i].second = i;
    }
    sort(P, P+n);
    p[0] = 0;
    for (int i = 1; i < n; i++)
        if (P[i-1].first == P[i].first)
            p[i] = p[i-1] ;
        else
            p[i] = p[i-1] +1;
    for (int i = 0; i < n; i++)
        a[P[i].second] = p[i];
    memset(cnt, 0, sizeof(int)*(n+10));
    int l = 0, ans = n, num = 0;
    int r = 0;
    while(r < n)
    {
        while(r < n && num <= p[n-1])
        {
            if(!cnt[a[r]])
                num++;
            cnt[a[r++]]++;
        }
        while(num > p[n-1])
        {
            if (--cnt[a[l++]] == 0)
                num--;
        }
        ans = min(ans, r-l+1);
    }
    printf("%d\n", ans);
    return 0;
```

[<–Back](blog/2015/02/13/chi-qu-fa/)
