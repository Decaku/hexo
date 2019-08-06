---
layout: ‘pw
title: Poj 2135 Farm Tour 费用流
date: 2019-02-04 15:30:13
tags: 费用流
categories: 费用流
---
# 描述:

给出起点和终点，要求从起点走到终点，再从终点回来，走的路最短，且来回走的路中不能有相同的边。

---
<!-- more -->
# 思路:

不要考虑来回，其实本质就是选择两条路，边权和最小，所以我们可以跑一个最大流是2的费用流。(选n条路就跑最大流是n的费用流)

本题所有边都是无向边。

---
```
#include<cstdio>
#include<queue>
#include<cstring>
//#include<bits/stdc++.h>
using namespace std;
const int inf=1e9;
const int maxn=2000;
const int maxm=11000;
int n,m,s,t;
int cnt=-1,tot,sum;
int dis[maxn],b[maxn],xb[maxn],flow[maxn],fa[maxn];
int head[maxm];
struct Edge
{
    int from,to,dis,nxt,w;
}edge[maxm*4];
void add_edge(int from,int to,int w,int dis)
{
   edge[++cnt].nxt=head[from];
   edge[cnt].from=from;
   edge[cnt].to=to;
   edge[cnt].w=w;
   edge[cnt].dis=dis;
   head[from]=cnt;
}
bool spfa()
{
    for(int i=0;i<=maxn;i++)
        dis[i]=inf;
    memset(b,0,sizeof(b));
    queue<int>que;
    while(!que.empty()) que.pop();
    memset(fa,-1,sizeof(fa));
    b[s]=1;
    dis[s]=0;
    fa[s]=0;
    flow[s]=inf;
    que.push(s);
    while(!que.empty())
    {
        int u=que.front();
        que.pop();
        b[u]=0;
        for(int i=head[u];i!=-1;i=edge[i].nxt)
        {
            int v=edge[i].to;
            if(edge[i].w&&dis[v]>dis[u]+edge[i].dis)
            {
                dis[v]=dis[u]+edge[i].dis;
                fa[v]=u;
                xb[v]=i;
                flow[v]=min(flow[u],edge[i].w);
                if(!b[v])
                {
                    b[v]=1;
                    que.push(v);
                }
            }
        }
    }
    return dis[t]<inf;
}
void max_flow()
{
    while(spfa())
    {
        int k=t;
        while(k!=s)
        {
            edge[xb[k]].w-=flow[t];
            edge[xb[k]^1].w+=flow[t];
            k=fa[k];
        }
        tot+=flow[t];
        sum+=flow[t]*dis[t];
    }
}


int main()
{
    memset(head,-1,sizeof(head));
    cnt=-1;
    scanf("%d %d",&n,&m);
    s=n+1;
    t=n+2;
    for(int i=1;i<=m;i++)
    {
       int a,b,c;
       scanf("%d %d %d",&a,&b,&c);
       add_edge(a,b,1,c);
       add_edge(b,a,0,-c);
       add_edge(b,a,1,c);
       add_edge(a,b,0,-c);
    }
    add_edge(s,1,2,0);
    add_edge(1,s,0,0);
    add_edge(n,t,2,0);
    add_edge(t,n,0,0);
    max_flow();
    printf("%d\n",sum);
}
```


