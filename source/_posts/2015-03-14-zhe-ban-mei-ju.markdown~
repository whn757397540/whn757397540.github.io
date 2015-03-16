---
layout: post
title: "折半枚举"
date: 2015-03-14 18:56:56 +0800
comments: true
categories: 技巧方法
---
这个，也是一种优化技巧，其思想来源于传统的双向搜索！

要点: 把要枚举的多个东西分成2部分分开来进行枚举，从而达到降低时间复杂度的目的，实际上是一种牺牲空间换取时间的方法，分成两部分来枚举的时候，就是先枚举一部分存起来，再根据存起来的这部分对剩下的一部分进行枚举。光说也看不出效果来，来看一道题。

<!--more-->

[POJ 2785 4 Values whose Sum is 0](http://poj.org/problem?id=2785)

就是给你4个序列，让你从每个序列中选一个数使得4个数和为0，问总共有几种方法（统一序列数值相同的数字区别看待）？

这个如果按照正常的方法直接枚举的话，复杂度是O(n^4)，但是如果我们先来枚举一半，看a+b+c+d = 0,则 c+d = -(a+b)，我们可以先枚举c和d的选取方法，总共是n^2种，复杂的自然就是n2，然后再把这几个数字存起来进行排序，之后再枚举a+b,利用a+b是c+d的相反数这个性质，利用二分搜索，可以再每次用logn的时间找出对应的c+d，而枚举a+b复杂度是n^2，综上，复杂度为O(n^2 logn).

参考代码理解一下就好。

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
const int maxn = 4010;

int a[maxn],b[maxn],c[maxn],d[maxn];
int cd[maxn*maxn];

int main()
{
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i++)
        scanf("%d%d%d%d", &a[i], &b[i], &c[i], &d[i]);
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            cd[i*n+j] = c[i]+d[j];
    sort(cd, cd+n*n);
    long long ans = 0;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
        {
            int ab = a[i] + b[j];
            ans += upper_bound(cd, cd+n*n, -ab) - lower_bound(cd, cd+n*n, -ab);
           /* if (ans != 0)
                cout<<a[i]<<" "<<b[i]<<" "<<ans<<endl;
            system("pause");*/
        }
    printf("%I64d\n", ans);
    return 0;
}
```

另外一个例子就是超大背包问题，就是背包问题，不过限制条件不适合用dp：1<=n<=40, 1<= w[i],v[i] <= 10^15, 1<W<10^15;

n比较小，就考虑能不能枚举，但是直接枚举复杂度是2n, 240铁定是不行的，所以，需要再优化。这时，就可以利用折半枚举，把所有的物品拆成2部分，先选一半，再选一半。实际上就是先枚举一半（2^20），假设枚举的这一半是w2,v2。存起来后，我们可以排除掉所有的w2[i]<=w2[j] && v2[i] >= v2[j]的j,按照w,v的字典序排序后很容易做到这一点。之后声誉的元素都满足w2[i]<w2[j]等价与v2[i]<v2[j]，我们要计算的是max{v2| w2<= W-w1},就只需要找到满足w2[i]<= W-w1[i]的最大的i就行，这个可以用二分搜索完成。复杂度是O（2^n/2 * n）,是可以在时限内完成的！

总结：对于枚举情况很多的时候，并且跟顺序有关的时候，可以折半，先枚举一半存起来，然后用二分查找的方法降低复杂度！