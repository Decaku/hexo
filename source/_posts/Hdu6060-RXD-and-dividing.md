---
title: Hdu6060 RXD and dividing
date: 2019-02-04 21:11:22
tags: dfs
categories: dfs
mathjax: true
---

# 描述:

$把树上2-n的结点分成k个集合且不能相交,然后每次取出一个集合 \\
所有点与1号结点形成连通分量把连通分量里的所有边权相加后加 \\
到ans上，求最大的ans。$

---
<!-- more -->
# 思路:

$一条边对于ans的贡献值应该是min\{k,k的子孙的数量\}*边权, \\
然后又是经典的统计子树了。$

---
```
#include<cstdio>
#include<algorithm>
#include<vector>
#include<iostream>
#include<queue>
#include<cstring>
#include<cmath>
using namespace std;

typedef long long ll;
const int maxn=1e6+5;

struct node
{
    int next,w;

};
vector<node>vec[maxn];
node tem;
int son[maxn],p[maxn];

void dfs(int v,int u)
{
    son[v]=1;
    int len=vec[v].size();
    for(int i=0; i<len; i++)
    {
        int t=vec[v][i].next;
        p[t]=vec[v][i].w;
        dfs(t,v);
        son[v]+=son[t];
    }
}

int main()
{
    int n,k,u,v,value;
    while(~scanf("%d%d",&n,&k))
    {
        for(int i=1; i<=n; i++)
        {
            vec[i].clear();
            son[i]=0;
            p[i]=0;
        }
        for(int i=1; i<=n-1; i++)
        {
            scanf("%d%d%d",&u,&v,&value);
            tem.next=v;
            tem.w=value;
            vec[u].push_back(tem);
        }
        dfs(1,0);
        ll ans=0;
        for(int i=2;i<=n;i++)
        {
            ans+=(ll)p[i]*min(son[i],k);
        }
        printf("%lld\n",ans);
    }
    return 0;
}
```


