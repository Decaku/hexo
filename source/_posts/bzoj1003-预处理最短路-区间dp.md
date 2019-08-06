---
title: bzoj1003 预处理最短路+区间dp
date: 2019-02-04 04:16:47
tags: [dp, 最短路]
categories: 动态规划
---
# 描述:

给一个图，让你跑n次最短路，第i次最短路有若干点是不能使用的，并且更改一次路线就又需要加k的额外花费，问跑完n次以后最小花费。

---
<!-- more -->
# 思路:

想到把n次最短路分割成多个区间最短路的和，一个区间内最短路是不变的，对于每种最短路，我们可以预处理，因为只有n方的区间，n最大100，点最多只有20个，先跑n方的spfa。设*dp[i][j]*表示第i次到第j次最短路的所有花费的最小值。那么答案就是*dp[1][n]*。

---
``` 
#include<bits/stdc++.h>
using namespace std;
const int maxn=200;
const int maxm=1000;
int cnt=-1,n,m,k,e;
int head[maxn],use[110][110],dist[maxn],dp[110][110];
bool inq[maxn];
struct Edge
{
    int nex;
    int to,w;
} edge[maxm];

void add_edge(int u,int v,int w)
{
    edge[++cnt].nex=head[u];
    edge[cnt].to=v;
    edge[cnt].w=w;
    head[u]=cnt;
}
bool check(int x,int s,int t)
{
    for(int i=s; i<=t; i++)
        if(!use[x][i])
            return false;
    return true;
}
int spfa(int s,int t)
{
    memset(dist,0x3f,sizeof(dist));
    memset(inq,false,sizeof(inq));
    queue<int>que;
    while(!que.empty())
        que.pop();
    que.push(1);
    dist[1]=0;
    inq[1]=true;
    while(!que.empty())
    {
        int u=que.front();
        que.pop();
        inq[u]=false;
        for(int i=head[u]; i!=-1; i=edge[i].nex)
        {
            int v=edge[i].to;
            if(check(v,s,t)&&dist[v]>dist[u]+edge[i].w)
            {
                dist[v]=dist[u]+edge[i].w;
                if(!inq[v])
                    inq[v]=true,que.push(v);
            }
        }
    }
    return dist[m]==0x3f3f3f3f ? 0x3f3f3f3f:dist[m]*(t-s+1);
}
void solve()
{
    for(int i=1; i<=n; i++)
    {
        for(int j=i; j<=n; j++)
            dp[i][j]=spfa(i,j);
    }
    int e;
    for(int len=2; len<=n; len++)
        for(int s=1; (e=s+len-1)<=n; s++)
            for(int p=s; p<s+len-1; p++)
            {
                dp[s][e]=min(dp[s][e],dp[s][p]+dp[p+1][e]+k);
            }
    printf("%d\n",dp[1][n]);
}

int main()
{
    while(~scanf("%d %d %d %d",&n,&m,&k,&e))
    {
        memset(head,-1,sizeof(head));
        cnt=-1;
        for(int i=1; i<maxm; i++)
            edge[i].nex=-1;
        for(int i=1; i<=e; i++)
        {
            int a,b,c;
            scanf("%d %d %d",&a,&b,&c);
            add_edge(a,b,c);
            add_edge(b,a,c);
        }
        memset(use,1,sizeof(use));
        int tem;
        scanf("%d",&tem);
        for(int i=1; i<=tem; i++)
        {
            int p,a,b;
            scanf("%d %d %d",&p,&a,&b);
            for(int j=a; j<=b; j++)
                use[p][j]=0;
        }
        solve();
    }
}

```
