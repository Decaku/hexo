---
title: Codeforces Beta Round 80 E
date: 2019-04-24 23:39:58
tags: [分块,离线]
categories: 分块

---

# 描述:

[传送门](https://codeforces.com/contest/104/problem/E)

$给一个序列s,多组询问,求 \sum_{i=a+k*b}^{i \leq n}s[i], \\
a和b也是输入的。$

---
<!-- more -->

#思路：

$这玩意看起来就没法数据结构加速,考虑b \geq \sqrt{n}时， \\
可以直接暴力，当b \leq \sqrt{n}时，我们直接预处理 \sqrt{n}种情况 \\
然后O(1)就行,但是这样空间是O(n \sqrt{n} )的，因为这题空间 \\
卡的紧所以可以离线询问排序后让空间O(n)。$

---

```
#include<bits/stdc++.h>
using namespace  std;
#define ll long long
const int maxn=3e5+5;
ll n,p,a[maxn],sum[maxn],ans[maxn];

struct Query
{
    ll x,y,id;
} q[maxn];

bool cmp(Query a,Query b)
{
    if(a.y==b.y)
        return a.id<b.id;
    return a.y<b.y;
}


int main()
{
    scanf("%lld",&n);
    for(int i=1; i<=n; i++)
        scanf("%lld",&a[i]);
    scanf("%lld",&p);
    for(int i=1; i<=p; i++)
    {
        scanf("%lld %lld",&q[i].x,&q[i].y);
        q[i].id=i;
    }
    sort(q+1,q+p+1,cmp);
    int l=1,r=1;
    while(1)
    {
        if(l>p) break;

        while(q[r+1].y==q[l].y) r++;
        if(q[r].y>sqrt(n))
        {
            for(int i=l;i<=r;i++)
            {
                ll tem=0;
                for(int j=q[i].x;j<=n;j+=q[i].y)
                {
                    tem+=a[j];
                }
                ans[q[i].id]=tem;
            }
            l=r+1;
            continue;
        }

        memset(sum,0,sizeof(sum));
        for(int i=n;i>=n-q[l].y+1;i--)
        {
            sum[i]=a[i];
        }


        for(int i=n-q[l].y;i>=1;i--)
        {
            sum[i]+=sum[i+q[l].y]+a[i];
        }

        for(int i=l; i<=r; i++)
        {
            ans[q[i].id]=sum[q[i].x];
        }
        l=r+1;
    }

    for(int i=1; i<=p; i++)
        printf("%lld\n",ans[i]);
}
```

