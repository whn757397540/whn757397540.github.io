---
layout: page
title: "source code of poj1064"
date: 2015-03-14 19:35
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
const int maxn = 11111;

double L[maxn];
int N,K;
double maxx;
const double eps = 1e-4;
bool canMake(double l)
{
    int res = 0;
    for(int i = 0; i < N; i++)
    {
        res += (int)(L[i]/l);
        if (res >= K)
            return true;
    }
    return false;
}

void solve()
{
    double l = 0, r = maxx;
   // cout<<l<<" "<<r<<" "<<eps<<endl;
    while((r-l) > eps)//如果范围足够小
    {

        double mid = (l+r)/2;
        if (canMake(mid))
            l = mid;
        else
            r = mid;
    }
    printf("%.2f\n", floor(r*100)/100);
}
int main()
{
    while(scanf("%d%d", &N, &K) == 2)
    {
        maxx = 0;
        for (int i = 0; i < N; i++)
        {
            scanf("%lf", &L[i]);
            maxx += L[i];
        }
        maxx /= K;

        solve();
    }
    return 0;
}
```

[<-Back](/blog/2015/02/13/er-fen-cha-zhao-ji-qi-ying-yong/)