---
layout: page
title: "Source code of poj2104"
date: 2015-03-14 20:01
comments: true
sharing: true
footer: true
---

桶分法(10s)
```c++
#include <cstdio>
#include <cstring>
#include <cstdlib>
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>

using namespace std;

const int maxn = 100010;
const int maxm = 5555;
const int B = 1000;

int n,m;
int a[maxn], b[maxn];
vector <int> bucket[maxn/B];

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; i++)
    {
        scanf("%d", &a[i]);
        b[i] = a[i];
        bucket[i/B].push_back(a[i]);
    }
    sort(b, b+n);
    for (int i = 0; i < n/B; i++)
        sort(bucket[i].begin(), bucket[i].end());
    for (int i = 0; i < m; i++)
    {
        int l,r,k;
        scanf("%d%d%d", &l, &r, &k);
        int lb = -1, ub = n-1;
        //二分答案
        while(ub - lb > 1)
        {
            int md = (lb + ub)>>1;
            int x = b[md];
            int tl = l - 1, tr = r, c = 0;
            //区间两端多出的部分，就是没有包括整个桶的那些部分，依次检查
            while(tl < tr && tl % B != 0)
                if (a[tl++] <= x) c++;
            while(tl < tr && tr % B != 0)
                if (a[--tr] <= x) c++;
            //从头到尾对每一个桶进行计算
            while(tl < tr)
            {
                int index = tl / B;
                c += upper_bound(bucket[index].begin(), bucket[index].end(), x) - bucket[index].begin();
                tl += B;
            }

            if (c >= k) ub = md;
            else lb = md;
        }
        printf("%d\n", b[ub]);
    }
```

线段树(6s)

```c++
#include <cstdio>
#include <cstring>
#include <cstdlib>
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>

using namespace std;

const int maxn = 100010;
const int maxm = 5555;

struct Tree{
    vector<int> dat;
    int l,r;
}T[maxn<<2];
int a[maxn],b[maxn];

void buildTree(int now, int l, int r)
{
    T[now].l = l;
    T[now].r = r;
    if (l == r)
    {
        T[now].dat.push_back(a[l]);
        return;
    }
    int lson = (now<<1), rson = (lson|1);
    int mid = (l+r)>>1;
    buildTree(lson, l, mid);
    buildTree(rson, mid+1, r);
    T[now].dat.resize(r-l+1);
    merge(T[lson].dat.begin(), T[lson].dat.end(), T[rson].dat.begin(), T[rson].dat.end(), T[now].dat.begin());
}

int query(int now, int l, int r, int x)
{
    if (r < T[now].l || T[now].r < l)//完全不相交
        return 0;
    if (l <= T[now].l && T[now].r <= r)//完全包含
        return upper_bound(T[now].dat.begin(), T[now].dat.end(), x) - T[now].dat.begin();
    int lson = now<<1, rson = lson|1;
    int lc = query(lson, l, r, x);
    int rc = query(rson, l, r, x);
 //   printf("[%d, %d] : [%d, %d] %d ;[%d, %d] %d\n", l, r, T[lson].l, T[lson].r, lc, T[rson].l, T[rson].r, rc);
   // system("pause");
    return lc+rc;
}

int n,m;
int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i++)
    {
        scanf("%d", &a[i]);
        b[i] = a[i];
    }
    sort(b+1, b+1+n);
    buildTree(1, 1, n);
    for (int i = 0; i < m; i++)
    {
        int x,y,k;
        scanf("%d%d%d", &x, &y, &k);
        int lb = 0, ub = n;
        while(ub - lb > 1)
        {
            int md = (lb+ub)>>1;
            int c = query(1,x,y,b[md]);
            if (c >= k) ub = md;
            else lb = md;
        }
        printf("%d\n", b[ub]);
    }
}
```
