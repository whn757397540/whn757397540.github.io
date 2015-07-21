---
layout: page
title: "hdu5294"
date: 2015-07-22 00:04
comments: true
sharing: true
footer: true
---

```c++
#include <iostream>
#include<cstdio>
#include<algorithm>
#include<cstring>
#include<cmath>
#include<queue>
#include <vector>
using namespace std;
#define INF 0x3f3f3f3f
#define MAXN 100100
#define MAXV 2222
#define MAXE 66666
const int mod = 1e9+7;
typedef pair<int,int> P;
struct edge
{
    int to,cap,next;
    edge() {}
    edge(int to, int cap,int next):to(to),cap(cap),next(next) {};
}E[MAXE<<1];
int head[MAXV];
int d[MAXV];
int gap[MAXV];
int cnt;
int n,S,T,all_pt;
vector<edge> G[MAXV];
vector<int> GG[MAXV];
vector<int> g[MAXV];

void init()
{
    cnt = 0;all_pt = n;
    memset(head, -1, sizeof(head));
    for(int i = 1; i <= n; i++)
    {
        GG[i].clear();
        G[i].clear();
        g[i].clear();

    }
}

void addEdge(int u, int v, int cap)
{
    E[cnt] = edge(v,cap,head[u]);
    head[u] = cnt++;
    E[cnt] = edge(u,0,head[v]);
    head[v] = cnt++;
}

void dijkstra(int s)
{
    priority_queue<P, vector<P>, greater<P> > que;
    fill(d+1, d+1+n, INF);
    d[s] = 0;
    que.push(P(0,s));
    while(!que.empty())
    {
        P p = que.top();
        que.pop();
        int v = p.second;
        if(d[v] < p.first) continue;
        for(int i = 0, sz = G[v].size(); i < sz; i++)
        {
            edge e = G[v][i];
            if(d[e.to] > d[v] + e.cap)
            {
                GG[e.to].clear();
                d[e.to] = d[v] + e.cap;
                que.push(P(d[e.to], e.to));
                GG[e.to].push_back(v);
            }
            else if (d[e.to] == d[v] + e.cap)
                GG[e.to].push_back(v);
        }
    }

}
bool vis[MAXV];
int minn;
void dfs1(int p)
{
    if(p == 1) return;
    if(vis[p]) return;
    vis[p] = true;
    for(int i = 0, sz = GG[p].size(); i < sz; i++)
    {
        g[GG[p][i]].push_back(p);
        dfs1(GG[p][i]);
    }
}

int dfs(int cur, int cur_cap)
{
    if(cur == T) return cur_cap;
    int min_dis = all_pt-1, temp_cap = cur_cap;
    for(int i = head[cur]; i != -1; i = E[i].next)
    {
        int cur_to = E[i].to, to_cap = E[i].cap;
        if(to_cap > 0)
        {
            if(d[cur_to] + 1 == d[cur])
            {
                int cap = min(temp_cap, E[i].cap);
                cap = dfs(cur_to, cap);
                temp_cap -= cap;
                E[i].cap -= cap;
                E[i^1].cap += cap;
                if(d[S] >= all_pt)
                    return cur_cap - temp_cap;
                if(temp_cap == 0)
                    break;
            }
            if(d[cur_to] < min_dis)
                min_dis = d[cur_to];
        }
    }
    if(temp_cap == cur_cap)
    {
        --gap[d[cur]];
        if(gap[d[cur]] == 0)
            d[S] = all_pt;
        d[cur] = min_dis+1;
        ++gap[d[cur]];
    }
    return cur_cap - temp_cap;
}

int isap()
{
    memset(gap, 0, sizeof(gap));
    memset(d, 0, sizeof(d));
    int ret = 0;
    gap[0] = all_pt;
    while(d[S] < all_pt)
        ret += dfs(S, INF);
    return ret;
}

void dfs2(int v, int deep)
{
    //cout<<1<<endl;
    if(v == n)
    {
        minn = min(minn, deep);
        //cout<<1<<endl;
        return;
    }
    for(int i = 0, sz = g[v].size(); i < sz; i++)
        dfs2(g[v][i], deep+1);
}
int main()
{
    //freopen("in.txt","r",stdin);
    int m;
    while(scanf("%d%d", &n, &m) != EOF)
    {
        init();
        for(int i = 0; i < m; i++)
        {
            int u,v,w;
            scanf("%d%d%d", &u, &v, &w);
            G[u].push_back(edge(v,w,0));
            G[v].push_back(edge(u,w,0));
        }
        dijkstra(1);
        // cout<<d[n]<<endl;
        memset(vis, false, sizeof(vis));
        minn = INF;
        dfs1(n);
//        for(int i = 1; i <= n; i++)
//        {
//            cout<<i<<": ";
//            for(int j = 0, sz = g[i].size(); j < sz; j++)
//                cout<<g[i][j]<<" ";
//            cout<<endl;
//        }
        for(int i = 1; i <= n; i++)
        {
            for(int j = 0, sz = g[i].size(); j < sz; j++)
                addEdge(i, g[i][j], 1);
        }
        S = 1, T = n;
        int flow = isap();
        minn = INF;
        dfs2(1, 1);
        printf("%d %d\n", flow, m-minn+1);
    }
    return 0;
}
```
