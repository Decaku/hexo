---
title: Poj 3281 Dining 最大流
date: 2019-02-04 15:44:51
tags: 最大流
categories: 最大流
---
# 描述:

有一些牛，食物，饮料，每头牛都有一个喜欢吃的食物和饮料集合，要求你找出一个最大匹配，能满足所有牛的需求。

---
<!-- more -->
# 思路:

拆点以后跑最大流。

---
```
#include<queue>
#include<cstdio>
#include<cstring>
#include<iostream>
//#include<bits/stdc++.h>
using namespace std;
const int maxn=500;
const int maxm=210000;
const int inf=1e9;
int cnt=-1,f,d,n,s,t;
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
        for(int i=1; i<=f+2*n+d+2; i++)
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
    for(int i=0; i<maxm*2; i++)
        edge[i].nxt=-1;
    memset(head,-1,sizeof(head));
    cnt=-1;
    scanf("%d %d %d",&n,&f,&d);
    s=f+2*n+d+1;
    t=f+2*n+d+2;
    for(int i=1; i<=n; i++)
    {
        int a,b,tem;
        scanf("%d %d",&a,&b);
        for(int j=1;j<=a;j++)
        {
            scanf("%d",&tem);
            add_edge(tem,f+i,1);
            add_edge(f+i,tem,0);
        }
        for(int j=1;j<=b;j++)
        {
            scanf("%d",&tem);
            add_edge(f+n+i,f+2*n+tem,1);
            add_edge(f+2*n+tem,f+n+i,0);
        }
        add_edge(f+i,f+n+i,1);
        add_edge(f+n+i,f+i,0);
    }
    for(int i=1;i<=f;i++)
    {
        add_edge(s,i,1);
        add_edge(i,s,0);
    }
    for(int i=1;i<=d;i++)
    {
        add_edge(f+2*n+i,t,1);
        add_edge(t,f+2*n+i,0);
    }
    printf("%d\n",Dinic());
}
```
