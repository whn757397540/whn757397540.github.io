---
layout: post
title: "图论总结(一)"
date: 2015-03-14 18:54:04 +0800
comments: true
categories: 图论
---
今天搞了一天的图论，重新熟悉了下Dijkstra，SPFA，PRIM和Kruskal算法，这里总结一下！

<!--more-->

先来几道例题（大部分是《挑战程序设计竞赛》中的例题）

1. [POJ3255 Roadblocks](http://poj.org/problem?id=3255)

这是一个次短路的题目，题意就是给你R条道路，总共有N个路口，问你从1号路口到N号路口的次短路（如果存在多个最短路，次短路就是比最短路长比其他所有路径都要短的路的长度）。求次短路有几种方法，这里我用的是《挑战程序设计竞赛》中提到的Dijkstra的方法，（至于求K短路的方法，这个等下一次总结再说！）

这个主要是要理解了Dijkstra的思想，我们想一下，从顶点1到某个顶点v的次短路要么是到顶点u的最短路加上u到v的边，要么是到顶点u的次短路加上顶点u到顶点v的边。所以更新的时候要用到到某个顶点u的最短路d[u]和到某个定点u的次短路dd[u],更新的规则如下：①先看d[u]能不能更新，需要更新则更新，同时也要更新次短路；②再看次短路能不能更新，需要更新则更新。（对于上面的两步操作，《挑战程序设计竞赛》上用了一种很巧妙的方法，具体请看代码）。这里说一下，用优先队列优化Dijkstra算法，在这个里面，要根据实际情况选择是用最短路入队还是用次短路入队，具体参考代码！代码如下：

```c++
#include <iostream>
#include <cstdio>
#include <cstring>
#include <set>
#include <algorithm>
#include <map>
#include <queue>
#include<vector>

using namespace std;
const int maxv = 5500;
const int maxe = 200100;
const int INF = 0x3fffffff;
struct Edge
{
    int from, to, cost, next;
    Edge() {}
    Edge(int from, int to, int cost, int next):from(from),to(to),cost(cost),next(next) {}
};
int head[maxv];
Edge E[maxe];
int nume;

void init()
{
    memset(head, -1, sizeof(head));
    nume = 0;
}

void addEdge(int u, int v, int cost)
{
    E[nume] = Edge(u,v,cost,head[u]);
    head[u] = nume++;
}

int d[maxv],dd[maxv];
priority_queue<pair<int, int>, vector<pair<int, int> >, greater<pair<int, int> > > que;

void dijkstra(int s, int e)
{
    fill(d, d+e+1, INF);
    fill(dd, dd+e+1, INF);
    d[s] = 0;
    que.push(make_pair(0,s));

    while (!que.empty())
    {
        pair<int,int> p = que.top();
        que.pop();
        int v = p.second;
        if (dd[v] < p.first) continue;
        for (int i = head[v]; i != -1; i = E[i].next)
        {
            int from = E[i].from, to = E[i].to, dis = E[i].cost;
            int tmp = p.first + dis;
            if (d[to] > tmp)
            {
                swap(d[to], tmp);
                que.push(make_pair(d[to], to));
            }
            if ( dd[to] > tmp && d[to] < tmp)
            {
                dd[to] = tmp;
                que.push(make_pair(dd[to], to));
            }
        }
    }
}

int main()
{
    //freopen("in.txt", "r", stdin);
    int n,m;
    while(scanf("%d%d", &n, &m)!=EOF)
    {
        init();
        for (int i = 0; i < m; i++)
        {
            int x,y,v;
            scanf("%d%d%d", &x, &y, &v);
            addEdge(x,y,v);
            addEdge(y,x,v);
        }
        dijkstra(1,n);
        printf("%d\n", dd[n]);
    }

}
```

2. [POJ 3169 Layout](http://poj.org/problem?id=3169)

这道题就需要思考怎么建图了，不看书我预计是不能够相处这么一种建图的方法的，因为做的少还不会。算是学习一下吧！

题意是给你N头牛，按照从近到远的顺序排列，牛与牛的距离可以是0，然后牛与牛的关系不同，有些牛之间的关系比较好，它们希望彼此之间的间距不要超过一定的距离；有些牛之间的关系比较差，它们希望彼此之间的间距不能小于一定的距离。先给出牛的个数N（编号从1到N，有序），给出你ML组关系好的牛的信息（AL,BL,DL）（AL<=BL）表示牛AL和牛BL之间的距离不能大于DL，给出你MD组关系差的牛的信息（AD,BD,DD)（AD <= BD）表示牛AD和牛BD之间的距离不能小于DD。问你在满足所有这些约束关系的情况中，1号牛和N号牛之间的最大距离。

这里我就借助书上说的思路来讲解一下了，也算是总结了。首先，设第i号牛的距离为的di,我们可以根据约束条件得出下面几个式子:①牛的编号是有序的，而且牛与牛之间的距离可以是0，那么有d[i]<=d[i+1];②对于每对关系好的牛(AL< BL)之间有最大距离限制DL，所以必须满足 d[BL] - d[AL] <= DL;③，对于每对关系比较差的牛（AD < BD）之间有最小距离限制DD，所以必须满足 DD <= d[BD] - d[AD]. 综合上面三组式子，其实可以列一个线性规划，但是显然，这个用计算机来实现很麻烦，现在问题的关键来了，怎么把它跟图联系起来呢？就现在的我来说，我是想不到该怎么办的！不过看了书之后，感觉书上的做法确实很敲秒，事实上，图论的题目难就难在建图，这里才是思考的关键，得慢慢练，下面说建图的方法：

我们来想一下，对于最短路d[u]和d[v]两个顶点，我们总有d[u] + w[u][v] >= d[v]成立（否则d[v]就不是最短路了），那么如果u = 1, v = n，我们就有d[1] + w[1][n] >= d[n]成立，即d[n] <= w[1][n],从这里我们可以看出，d[n]的最大值就是w[1][n]，也就是从1到n的最短距离。

对比一下上面的分析，不难看出其实式子的模样很像，看，对于①，我们可以写成d[i+1] + 0 >= d[i]， 所以我们需要建一个从i+1指向i的长度为0的边；对于②， 我们可以写成d[AL]+DL >= d[BL]，我们应该建一个从AL指向BL的长度为DL的边；对于③，我们可以写成d[BD] - DD >= d[AD],所以我们应该建一个从BD指向AD的长度为-DD的边。从第对牛的关系的分析可知，这里d[u]代表牛u和牛1的距离，所以我们需要求的是d[n]的最大值，而通过对最短路的分析我们可知，d[n]的最大值也就是从1到n的最短距离。（看起来是不是好怪的样子qq_pp），所以我们建好图之后只需要求出最短路d[n]就行！

由于存在负边，所以我们这里用的是SPFA算法（Dijkstra算法不能处理负边的情况），我的代码如下：

```c++
#include <iostream>
#include <cstdio>
#include <cstring>
#include <set>
#include <algorithm>
#include <map>
#include <queue>
#include<vector>

using namespace std;
const int maxv = 1010;
const int maxe = 20200;
const int INF = 0x3fffffff;
struct Edge{
    int from,to,cost,next;
    Edge(){};
    Edge(int from, int to, int cost, int next) : from(from), to(to), cost(cost), next(next){}
}E[maxe];
int head[maxv];
queue<int> que;
int nume;

void init()
{
    nume = 0;
    memset(head, -1, sizeof(head));
}

void addEdge(int u, int to, int cost)
{
    E[nume] = Edge(u,to,cost,head[u]);
    head[u] = nume++;
}

int d[maxv];
int cnt[maxv];
bool inq[maxv];
bool SPFA(int s, int e)
{
    fill(d, d+e+1, INF);
    memset(inq, 0, sizeof(inq));
    memset(cnt, 0, sizeof(cnt));
    //cout<<1<<endl;
    d[s] = 0;
    inq[s] = true;
    que.push(s);
    while(!que.empty())
    {
        int u = que.front();
        que.pop();
        inq[u] = false;
        for (int i = head[u]; i != -1; i = E[i].next)
        {
            int to = E[i].to, w = E[i].cost;
            if (d[u] < INF && d[to] > d[u] + E[i].cost)
            {
                d[to] = d[u] + E[i].cost;
                if (!inq[to])
                {
                    que.push(to);
                    inq[to] = true;
                    if (++cnt[to] > e)
                            return false;
                }
            }
           // cout<<
        }
       // cout<<u<<endl;
    }
    return true;
}

int main()
{
     int n, ml, md;
     while(scanf("%d%d%d", &n, &ml, &md) == 3)
     {
         init();
         for (int i = 1; i < n; i++)
            addEdge(i+1, i, 0);
         for (int i = 0; i < ml; i++)
         {
             int a,b,dis;
             scanf("%d%d%d", &a, &b, &dis);
             addEdge(a, b, dis);
         }
         for (int i = 0; i < md; i++)
         {
             int a,b,dis;
             scanf("%d%d%d", &a, &b, &dis);
             addEdge(b, a, -dis);
         }
       //  cout<<1<<endl;
         if (SPFA(1, n))
         {
             if (d[n] < INF)
                printf("%d\n", d[n]);
             else
                printf("%d\n", -2);
         }
         else
            printf("%d\n", -1);

     }
}
```

以上是Dijkstra算法和SPFA算法的两个例子，这里总结一下二者的区别，首先，从思想上来说，Dijkstra算法是 从一点开始依次更新每一点，每次取距离初始点最近的一个点对接下来的点进行更新；而SPFA是对每一个节点进行多次松弛操作，直到所有的节点都是最短路为止！从实现上来看，Dijkstra的算法代码跟SPFA是非常像的，在我看来，不同点就在于：第一，DJ用的是优先队列，而SPFA用的是普通FIFO队列；第二，DJ每个元素只入队一次，而SPFA可以入队多次（据说一般不超过2次）（好了，我承认，这是在刘汝佳紫书上看的，不过很对不是？）。然后，一般情况下，DJ是比较稳定的，是SPFA的时间复杂度不够稳定，所以，如果不是特殊需要（比如有负边或者负环啦），最好还是用时间比较稳定的DJ。（PS:外国不承认SPFA，只认Bellman-ford，而SPFA就是在Bellman-ford的基础上加了个队列优化。）

最后对于最短路，再说一下路径还原的事情，以DJ为例，我们要还原出最短路来，只需要在更新最短路的过程中加上一个队父亲节点的更新（记录最短路上to节点的父亲节点是最短路上的from节点），最后从目标节点回溯回去即可。正好前些天CF的E题就用到了最短路的还原，实在是敲累了，思想说了，直接挂代码了（这里的代码是我最开始根据最短路DJ的思想敲的，没有板子，姿势可能有点丑，不过也懒得改了）

[CF 507E](http://codeforces.com/contest/507/problem/E)

题意简单说以下，就是给你一个图，这个图中每条路的路径长度都是1，每条路有通(1)和不通(0)两种状态。你要从1号点到n号点，而且你所走的路径上的所有路都要修好（就是如果有0要变成1），其他的所有路都要拆断（就是所有不属于你走的路径上的路如果有1都要变成0），要求走最短路（距离最短），如果有多个最短路，走需要作出的改变最少的那一种路（如果还是有多种，任何一种都行）。要求输出最少的改变数和每个改变的信息！代码如下：

```c++
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <cstring>
#include <algorithm>
#include <queue>
#include <vector>

using namespace std;
const int maxv = 200100;
const int maxe = 200100;
struct Edge{
    int from, to, state, next;
    Edge(){}
    Edge(int from, int to, int state, int next):from(from),to(to),state(state),next(next){}
};
int head[maxv];
Edge E[maxe];
int v,cntedge;

void init()
{
    memset(head, -1, sizeof(head));
    cntedge = 0;
}

void addEdge(int u, int v, int state)
{
    E[cntedge] = Edge(u,v,state,head[u]);
    head[u] = cntedge++;
}

int d[maxv],father[maxv],dd[maxv];
priority_queue<pair<int, int>, vector<pair<int, int> >, greater<pair<int, int> > > que;

void dijkstra(int s, int e)
{
   fill(d, d+e+1, maxv+1);
   fill(dd, dd+e+1, 0);
   fill(father, father+e, -1);
   d[s] = 0;
   pair<int, int> now = make_pair(0, s);
   while (now.second != e)
   {
        for (int v = head[now.second]; v != -1; v = E[v].next)
        {
            int from = E[v].from, to = E[v].to;
            if (d[to] > d[from] + 1 || d[to] == d[from] + 1 && dd[to] < dd[from] + E[v].state)
            {
                dd[to] = dd[from] + E[v].state;
                d[to] = d[from] + 1;
                father[to] = v;
                que.push(make_pair(d[to], to));
            }
          //  cout<<to<<d[to]<<endl;
           // cout<<v<<" "<<to<<endl;
        }
      //  cout<<now.second<<endl;
        now = que.top();
        // cout<<now.second<<endl;
        que.pop();
        //system("pause");
   }
}
bool judge[maxe];
typedef pair<int, int> P;
typedef pair<P, int> PP;
vector<PP> ans;
void getAns(int aim)
{
    memset(judge, 0, sizeof(judge));
    ans.clear();
    int tp = aim;
    while (father[tp] != -1)
    {
        int e1 = father[tp];
        tp = E[e1].from;
        judge[e1] = true;
        judge[e1^1] = true;
    }
    for (int i = 0; i < cntedge; i++)
    {
        if (judge[i])
        {
            if (E[i].state == 0 && !(i&1))
                ans.push_back(make_pair(make_pair(E[i].from, E[i].to), 1));
        }
        else
        {
            if (E[i].state == 1 && !(i&1))
                ans.push_back(make_pair(make_pair(E[i].from, E[i].to), 0));
        }
    }
}
int main()
{
    //freopen("in.txt", "r", stdin);
    int n,m;
    while (scanf("%d%d", &n, &m) == 2)
    {
        init();
        for (int i = 0; i < m; i++)
        {
            int x,y,v;
            scanf("%d%d%d", &x, &y, &v);
            addEdge(x,y,v);
            addEdge(y,x,v);
           // cout<<0<<endl;
        }
        while (!que.empty())
            que.pop();
        //cout<<0<<endl;
        dijkstra(1,n);
       // cout<<1<<endl;
        getAns(n);
       // cout<<2<<endl;
        printf("%d\n", ans.size());
        for (int i = 0; i < ans.size(); i++)
        {
            printf("%d %d %d\n", ans[i].first.first, ans[i].first.second, ans[i].second);
        }

    }

}
```

最短路的总结暂时就告一段落了，这也是今天总结最主要的一部分，下面简单来说一说最小生成树！还是先来看例题。

1. [POJ 3723 Conscription](http://poj.org/problem?id=3723)

题意：招募女兵n人，男兵m人，每人需要花费10000快，某些女兵同某些男兵之间有一定的亲密度（1–9999），招募某个人的费用是10000-（已经招募了的人之中和自己亲密度的最大值）。问你征募所有人所需的最小费用是多少。

首先，所有人都要招募，所以我就先把所有人都招募进来，总共花费(n+m)*10000，但是有些人花费的要少一些，我们只需要把少的这些从总费用里扣除掉就行。少多少实际上可以用亲密度的最大生成树来表示（或者有可能是最大生成森林），当然，如果权值取亲密度的相反数，那么就是最小生成树了。这里用Kurskal算法比较好，一是点比较多，20000，二是PRIM求最小生成森林也比较麻烦，当然，主要原因是第一点。不多说了，上代码：

```c++
#include <iostream>
#include <cstdio>
#include <cstring>
#include <set>
#include <algorithm>
#include <map>
#include <queue>
#include<vector>

using namespace std;
const int maxv = 20020;
const int maxe = 100100;
const int INF = 0x3fffffff;
struct Edge{
    int x, y, d;
    bool operator <(Edge p)const{return d < p.d;}
};

int n,m,r;
Edge E[maxe];

int father[maxv];

void init(int n)
{
    for (int i = 0; i < n; i++)
        father[i] = i;
}
int getfather(int i)
{
    if (father[i] == i)
        return i;
    return father[i] = getfather(father[i]);
}

bool judge(int i, int j)
{
    int a = getfather(i), b = getfather(j);
    if (a == b)
        return true;
    return false;
}

void getUnion(int i, int j)
{
    int a = getfather(i), b = getfather(j);
    father[a] = b;
}

int main()
{
     int t;
     scanf("%d", &t);
     while(t--)
     {
         scanf("%d%d%d", &n, &m, &r);
         init(n+m);
         for (int i = 0; i < r; i++)
         {
             int x,y,d;
             scanf("%d%d%d", &x, &y, &d);
             E[i] = (Edge){x, y+n, -d};
         }
         sort(E, E+r);
         int ans = (n+m)*10000;
         for (int i = 0; i < r; i++)
         {
             if (!judge(E[i].x, E[i].y))
             {
                 ans += E[i].d;
                 getUnion(E[i].x,E[i].y);
             }
         }
         printf("%d\n", ans);
     }
}
```

最后，还找了一道PRIM的练习题，如下

[POJ 1251 Jungle Roads](http://poj.org/problem?id=1251)

赤果果的一道最小生成树的题目，只不过读入的方式不太一样而已。这里只有不到30个点，所以用PRIM还是一点问题都没有的。还是挂一下代码吧！

PRIM代码

```c++
#include <iostream>
#include <cstdio>
#include <cstring>
#include <set>
#include <algorithm>
#include <map>
#include <queue>
#include<vector>

using namespace std;
const int maxv = 1010;
const int maxe = 20200;
const int INF = 0x3fffffff;
typedef pair<int, int> P;

int a[30][30];
int n,m;
bool vis[30];
int PRIM()
{
    fill(vis, vis+n, false);
    int res = 0;
    priority_queue<P, vector<P>, greater<P> > que;
    for (int i = 0; i < n; i++)
        if (a[0][i] < INF)
            que.push(make_pair(a[0][i], i) );
    vis[0] = true;
    while(!que.empty())
    {
        P u = que.top();que.pop();
        if (vis[u.second]) continue;
        res += u.first;
        vis[u.second] = true;
        for (int i = 0; i < n; i++)
        {
            if (!vis[i])
                que.push(make_pair(a[u.second][i], i));
        }
    }
    return res;
}
int main()
{
    while (scanf("%d", &n))
    {
        if (n == 0)
            break;
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                a[i][j] = INF;

        for (int i = 0; i < n-1; i++)
        {
            char c;
            scanf("%s%d", &c, &m);
            int x = c-'A';
            for (int j = 0; j < m; j++)
            {
                char ch;
                int len;
                scanf("%s%d", &ch, &len);
                a[x][ch-'A'] = len;
                a[ch-'A'][x] = len;
            }
         }
         int res = PRIM();
         printf("%d\n",res);
     }
}
```

KRUSKAL代码

```c++
#include <iostream>
#include <cstdio>
#include <cstring>
#include <set>
#include <algorithm>
#include <map>
#include <queue>
#include<vector>

using namespace std;
const int maxv = 30;
const int maxe = 1000;
const int INF = 0x3fffffff;
typedef pair<int, int> P;
typedef pair<int, P> Edge;

Edge E[maxe];
int n,m;
int nume;
int father[maxv];

void init(int n)
{
    for (int i = 0; i < n; i++)
        father[i] = i;
    nume = 0;
}

int getfather(int i)
{
    if (father[i] == i)
        return i;
    return father[i] = getfather(father[i]);
}

bool judge(int i, int j)
{
    int a = getfather(i), b = getfather(j);
    return a == b;
}

void getUnion(int i, int j)
{
    int a = getfather(i), b = getfather(j);
    father[a] = b;
}

int Kruskal()
{
    sort(E, E+nume);
    int res = 0;
    for (int i = 0; i < nume; i++)
    {
        P tp = E[i].second;
        if (!judge(tp.first, tp.second))
        {
            res += E[i].first;
            getUnion(tp.first, tp.second);
        }
    }
    return res;
}
int main()
{
    while (scanf("%d", &n))
    {
        if (n == 0)
            break;
        init(n);

        for (int i = 0; i < n-1; i++)
        {
            char c;
            scanf("%s%d", &c, &m);
            int x = c-'A';
            for (int j = 0; j < m; j++)
            {
                char ch;
                int len;
                scanf("%s%d", &ch, &len);
                E[nume++] = make_pair(len, make_pair(x, ch-'A'));
            }
        }

        int res = Kruskal();
        printf("%d\n",res);
     }
}
```

总结一下，一般情况下，就用Kruskal吧，除非是稠密图（点特别少，边特别多的时候），我确实Kruskal掌握地比PRIM好一些，但是今天敲了下PRIM，确实是PRIM敲起来更加简单，还是要知道怎么用的，必要的时候要能够会用！

好了，今天一天干的事情大概都在上面了，真是，一个这个总结都写了2个多小时，╮(╯▽╰)╭，忧伤！