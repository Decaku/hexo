---
title: Poj4045 Power Station
date: 2019-02-05 01:56:25
tags: 树形dp
categories: 动态规划
mathjax: true

---
# 描述:

找到树上一个结点,树上其它结点与它距离之和最小。

---
<!-- more -->
#思路:

$枚举每一条树边,儿子的值可以由父亲转移过来, \\
统计完所有结点以后比较即可。所以需要做两遍dfs, \\
第一遍统计每个结点子树的数量和根节点的值。 \\
第二遍dfs更新其它结点的值。$

---
```
#include<vector>
#include<cstring>
#include<cstdio>
//#include<bits/stdc++.h>
using namespace std;
const int maxn=5e4+5;
const int inf=1e9;
vector<int>ans;

struct Edge
{
    int to,nex;
} edge[maxn*2];

int head[maxn],cnt,siz[maxn],dis[maxn],tem;
int n,I,R;

void add_edge(int u,int v)
{
    edge[++cnt].to=v;
    edge[cnt].nex=head[u];
    head[u]=cnt;
}

void dfs1(int u,int f,int dep)
{
    siz[u]=1;
    for(int i=head[u]; i!=-1; i=edge[i].nex)
    {
        int v=edge[i].to;
        if(v==f)
            continue;
        tem+=dep;
        dfs1(v,u,dep+1);
        siz[u]+=siz[v];
    }
}

void dfs2(int u,int f)
{
    if(u!=1)
        dis[u]=dis[f]-siz[u]+n-siz[u];
    for(int i=head[u]; i!=-1; i=edge[i].nex)
    {
        int v=edge[i].to;
        if(v==f)
            continue;
        dfs2(v,u);
    }
}
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        ans.clear();
        memset(head,-1,sizeof(head));
        cnt=-1;
        tem=0;
        scanf("%d %d %d",&n,&I,&R);
        for(int i=1; i<n; i++)
        {
            int u,v;
            scanf("%d %d",&u,&v);
            add_edge(u,v);
            add_edge(v,u);
        }
        dfs1(1,0,1);
        dis[1]=tem;
        dfs2(1,0);
        int mi=inf;
        for(int i=1; i<=n; i++)
        {
            if(dis[i]<mi)
            {
                mi=dis[i];
                ans.clear();
                ans.push_back(i);
            }
            else if(dis[i]==mi)
                ans.push_back(i);
        }
        printf("%lld\n",1ll*mi*I*I*R);
        printf("%d",ans[0]);
        for(int  i=1; i<ans.size(); i++)
            printf(" %d",ans[i]);
        printf("\n");
        printf("\n");
    }
}
```
