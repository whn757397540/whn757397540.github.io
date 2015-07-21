---
layout: page
title: "hdu5299"
date: 2015-07-22 00:04
comments: true
sharing: true
footer: true
---

```c++
#include <iostream>
#include <cstdio>
#include <cstring>
#include <cstdlib>
#include <algorithm>
#include <cmath>
#include <string>
#include <vector>
typedef long long ll;
using namespace std;
const double PI = acos(-1);
const double eps = 1e-9;
const int maxn = 22222;
int n;
int x[maxn],y[maxn],r[maxn];
vector<pair<int,int> > events;
vector<int> G[maxn];
bool vis[maxn];
bool inside(int i, int j)
{
    int dx = x[i] - x[j], dy = y[i]-y[j];
    return dx*dx+dy*dy <= r[j]*r[j];
}

int dfs(int v)
{
    if(G[v].size() == 0) return 0;
    int res = 0;
    for(int i = 0, sz = G[v].size(); i < sz; i++)
    {
        res ^= (dfs(G[v][i])+1);
    }
    return res;
}
void solve()
{
    events.clear();
    for(int i = 0; i <= n; i++)
        G[i].clear();
    for(int i = 0; i < n; i++)
        events.push_back(make_pair(x[i]-r[i], i));

    sort(events.begin(), events.end());
    for(int i = 0, sz = events.size(); i < sz; i++)
    {
        int j = i-1;
        //cout<<events[i].second<<endl;
        int id = events[i].second;
        while(j >= 0 && !vis[events[j].second%n]&&!inside(id, events[j].second)) j--;
       // cout<<id<<" "<<j<<endl;
        if(j >= 0)
            G[events[j].second%n].push_back(id);
        else
            G[n].push_back(id);

    }
    printf("%s\n", dfs(n)?"Alice":"Bob");
}
int main()
{
    int T;
    //freopen("in.txt", "r", stdin);
    cin>>T;
    while(T--)
    {
        scanf("%d",&n);
        //cout<<n<<endl;
        for(int i = 0; i < n; i++)
            scanf("%d%d%d", &x[i], &y[i], &r[i]);
        solve();
    }
}
```

