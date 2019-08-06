---
title: Poj 3041 Asteroids 最小点覆盖
date: 2019-02-04 15:36:45
tags: [最小点覆盖, 最大流]
categories: 最大流
---
# 描述:

有一个500*500的网格图，格点上有一些点，每次可以消除一行或者一列上所有的点，问消除所有的点最少需要几次。

---
<!-- more -->
# 思路:

因为要消除一个点,只有2种方式，并且一行或者一列多次消除是没有意义的。可以考虑构建一个二分图，左边是所有的行，右边是所有的列，把待消除的点作为边，一个点会唯一连一条边，那么题意转化为了，选择二分图上的点，把与这个点连的边加入一个集合，这个集合应该包含了所有边，最少选择的点的个数，问题转化为了最小点覆盖。

二分图里最小点覆盖等于最大匹配，跑一遍最大流即可。

---
```
#include<queue>
#include<cstdio>
#include<cstring>
//#include<bits/stdc++.h>
using namespace std;
const int maxn=1100;
const int maxm=260000;
const int inf=1e9;
int cnt=-1,n,k,s,t;
int head[maxn],dep[maxn],cur[maxn];

struct Edge
{
    int nxt;
    int to,w;
} edge[maxm*2];

void add_edge(int u,int v,int w)
{
    edge[++cnt].nxt=head[u];
    edge[cnt].to=v;
    edge[cnt].w=w;
    head[u]=cnt;
}
bool bfs()
{
    queue<int>que;
    while(!que.empty())que.pop();
    memset(dep,0,sizeof(dep));
    dep[s]=1;
    que.push(s);
    while(!que.empty())
    {
        int u=que.front();
        que.pop();
        for(int i=head[u]; i!=-1; i=edge[i].nxt)
        {
            int v=edge[i].to;
            if(!dep[v]&&edge[i].w)
            {
                dep[v]=dep[u]+1;
                que.push(v);
            }
        }
    }
    if(dep[t]>0)
        return 1;
    return 0;
}
int dfs(int u,int flow)
{
    if(u==t) return flow;
    for(int &i=cur[u]; i!=-1; i=edge[i].nxt)
    {
        int v=edge[i].to;
        if((dep[v]==dep[u]+1)&&edge[i].w)
        {
            int d=dfs(v,min(flow,edge[i].w));
            if(d>0)
            {
                edge[i].w-=d;
                edge[i^1].w+=d;
                return d;

            }
        }
    }
    return 0;
}
int Dinic()
{
    int ans=0;
    while(bfs())
    {
        for(int i=1; i<=2*n+2; i++)
            cur[i]=head[i];
        while(int d=dfs(s,inf))
        {
            ans+=d;
        }
    }
    return ans;
}


int main()
{
    //建图 左边点1-N(行) 右边点N+1， 2N  源点2N+1,汇点2N+2;
    for(int i=0;i<maxm*2;i++)
        edge[i].nxt=-1;
    scanf("%d %d",&n,&k);
    memset(head,-1,sizeof(head));
    cnt=-1;
    for(int i=1;i<=k;i++)
    {
        int x,y;
        scanf("%d %d",&x,&y);
        add_edge(x,y+n,1);
        add_edge(y+n,x,0);
    }
    for(int i=1;i<=n;i++)
    {
        add_edge(2*n+1,i,1);
        add_edge(i,2*n+1,0);
        add_edge(i+n,2*n+2,1);
        add_edge(2*n+2,i+n,0);
    }
    s=2*n+1;
    t=2*n+2;
    printf("%d\n",Dinic());
}
```