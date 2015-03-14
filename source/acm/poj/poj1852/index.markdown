---
layout: page
title: "Source of poj1852"
date: 2015-02-17 19:22
comments: true
sharing: true
footer: true
---

```c++
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int maxn = 100010;
const int INF = 0x3f3f3f3f;
int le, n;
int a[maxn];
int main()
{
    int t;
    cin>>t;
    while(t--)
    {
        int maxx = 0, minn =0;
        scanf("%d%d", &le, &n);
        for (int i = 0; i < n; i++)
            scanf("%d", &a[i]);
        for (int i = 0; i < n; i++)
        {
            maxx = max(maxx, max(a[i], le - a[i]));
            minn = max(minn, min(a[i], le - a[i]));
        }
        cout<<minn<<" "<<maxx<<endl;
    }
}
```

[<-Back](/blog/2015/02/17/dan-xing-peng-zhuang-wen-ti/)