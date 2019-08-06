---
title: Hdu1520 Anniversary party
date: 2019-02-05 01:49:09
tags: 树形dp
categories: 动态规划
mathjax: true

---
# 描述:

从树上选择一些结点,使被选择的结点的权值和最大,儿子与父亲结点最多选一个。

---
<!-- more -->
# 思路:

$设dp[0][i],表示以i为根节点,且i结点不选能取得的最大值, \\
dp[1][i]表示以i为根节点且i结点选择的最大值,答案为  \\
max\{dp[0][1],dp[1][1]\}。 \\
转移方程是 dp[0][u]=dp[0][u]+max\{dp[1][v],dp[0][v]\}; \\
dp[1][u]+=dp[0][v]; 其中v是u的儿子。$

---
```
#include<bits/stdc++.h>
using namespace std;
const int maxn=6000+10;
int head[maxn],cnt,value[maxn];
int dp[2][maxn];

struct Edge
{
    int to,nex;
} edge[maxn*2];
void add_edge(int u,int v)
{
    edge[++cnt].to=v;
    edge[cnt].nex=head[u];
    head[u]=cnt;
}

void dfs(int u,int f)
{
    for(int i=head[u]; i!=-1; i=edge[i].nex)
    {
        int v=edge[i].to;
        if(v==f) continue;
        dfs(v,u);
        dp[0][u]=dp[0][u]+max(dp[1][v],dp[0][v]);
        dp[1][u]+=dp[0][v];
    }
}

int main()
{
    int n;
    while(~scanf("%d",&n))
    {
        memset(head,-1,sizeof(head));
        cnt=-1;
        for(int i=1; i<=n; i++)
            scanf("%d",&value[i]);
        int u,v;
        while(~scanf("%d %d",&u,&v))
        {
            if(!u&&!v) break;
            add_edge(u,v);
            add_edge(v,u);
        }
        memset(dp[0],0,sizeof(dp[0]));
        for(int i=1; i<=n; i++)
            dp[1][i]=value[i];
        dfs(1,0);
        printf("%d\n",max(dp[0][1],dp[1][1]));
    }
}
```


