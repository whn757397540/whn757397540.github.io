---
layout: page
title: "Source code of poj3684"
date: 2015-03-17 19:23
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
const double g = 10.0;
int n,h,r,t;
int c;
double y[maxn];
int main()
{
    cin>>c;
    while(c--)
    {
        cin>>n>>h>>r>>t;
        for(int i = 0; i < n; i++)
        {
            double H = h;
            double T = sqrt(2*H/g);
            double t1 = t - i;
            if (t1 < 0)
            {
                y[i] = H;
                continue;
            }
            int k = (int)(t1/T);
            double tt = t1 - k*T;
            if (k & 1)
                y[i] = H - 0.5*g*(T-tt)*(T-tt);
            else
                y[i] = H - 0.5*g*tt*tt;
        }
        sort(y, y+n);
        for (int i = 0; i < n; i++)
            printf("%.2f%c", y[i]+2*r*i/100.0, i == n-1 ? '\n' : ' ');
    }
    return 0;
}
```

[<-Back](/blog/2015/02/17/dan-xing-peng-zhuang-wen-ti/)
