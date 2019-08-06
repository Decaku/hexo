---
title: Poj 3522 Slim Span
date: 2019-02-04 23:06:17
tags: 最小差值生成树
categories: 最小生成树
mathjax: true

---
# 描述:

找一棵生成树，且生成树中边权最大差值尽可能小。

---
<!-- more -->
# 思路:

$把边权从小到大排序，然后依次枚举生成树中的最小权重，更新差值, \\
找到答案，算法复杂度为E\*log(V)\*E。$

---
```
#include <iostream>
#include <cstring>
#include <cstdlib>
#include <algorithm>
#include <cstdio>
#include <string>

using namespace std;
int p[105];
int Find(int x)
{
    return p[x]==x?x:p[x]=Find(p[x]);
}
void join(int x,int y)
{
    int px=Find(x),py=Find(y);
    if(px!=py)
        p[px]=py;
}
struct node
{
    int u,v,w;
} e[6000];
bool cmp(node x,node y)
{
    return x.w<y.w;
}

int main()
{
    int n,m,t;
    bool flag;
    while(cin>>n>>m&&(n+m))
    {
        for(int i=1; i<=m; i++)
        {
            cin>>e[i].u>>e[i].v>>e[i].w;
        }
        sort(e+1,e+m+1,cmp);
        flag=false;
        int ans=1e7;
        for(int i=1; i<=m; i++)
        {
            t=0;
            int tem;
            for(int k=0; k<105; k++)
                p[k]=k;
            for(int j=i; j<=m&&t<n-1; j++)
            {
                int x=Find(e[j].u);
                int y=Find(e[j].v);
                if(x!=y)
                {
                    p[x]=y;
                    t++;
                    tem=j;
                }
            }
            if(t==n-1)
            {
                flag=true;
                ans=min(ans,e[tem].w-e[i].w);
            }
        }
        if(flag)
            cout<<ans<<endl;
        else cout<<-1<<endl;
    }
}
```