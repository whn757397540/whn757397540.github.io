---
layout: page
title: "poj3276"
date: 2015-03-14 19:46
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

char s[maxn];
int f[maxn];
int n;

int solve(int k)
{
    fill(f, f+n, 0);
    int sum = 0;
    int res = 0;
    for (int i = 0; i + k -1 < n; i++)
    {
        if (s[i] == 'F')
        {
            if (i-k >= 0)
                sum -= f[i-k];
            f[i] = (sum&1);
        }
        else if (s[i] == 'B')
        {
            if (i-k >= 0)
                sum -= f[i-k];
            f[i] = !(sum&1);
           // cout<<sum<<" "<<(sum&1)<<" "<<!(sum&1)<<endl;
        }
        res += f[i];
        sum += f[i];
      //  cout<<f[i]<<" "<<endl;
        //system("pause");
    }
    for (int i = n-k+1; i < n; i++)
    {
         if (s[i] == 'F')
        {
            if (i-k >= 0)
                sum -= f[i-k];
            f[i] = (sum&1);
        }
        else if (s[i] == 'B')
        {
            if (i-k >= 0)
                sum -= f[i-k];
            f[i] = !(sum&1);
        }
        if (f[i])
            return INF;//无解，返回最大值
    }
    return res;
}
int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i++)
        scanf("%s", &s[i]);
    int ansk = n, ansm = n;
    for (int k = 1; k <= n; k++)
    {
        int tmp = solve(k);
        //cout<<tmp<<" "<<k<<endl;
        if (tmp < ansm)
        {
            ansk = k;
            ansm = tmp;
        }
    }
    printf("%d %d\n", ansk, ansm);
   // system("pause");
    return 0;
}
```