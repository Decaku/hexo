---
title: Poj 2186 Popular Cows
date: 2019-02-04 22:52:11
tags: Tarjan缩点
categories: Tarjan缩点
mathjax: true
---
# 描述:

$有若干cows，如果a崇拜b，b崇拜c那么a崇拜c，求有多少cows \\
被其他的所有cows崇拜。$

---
<!-- more -->
# 思路:

$建立有向图，然后缩点，需要找的点就是出度为0的点。 \\
证明:若有一点a出度不为0且a到b有一条边，由于是DAG \\
可知不存在从b到a的路径，所以a不是所求点，又发现若 \\
出度为0的点数量大于1时必然不满足题意,这些点之间不 \\
存在有向边。$

---
```
#include<cstdio>
#include<cstring>
#include<iostream>
using namespace std;
const int maxn=20005;
const int maxm=50005;
struct Edge
{
    int to,next;
} edge[maxm];

int head[maxn],tot;
int index,top;
int Low[maxn],Dfn[maxn],Stack[maxn],Belong[maxn];
int mark[maxn];

int scc;
bool Instack[maxn];
int num[maxn];
int n,m,x,y;

void addedge(int u,int v)
{
    edge[tot].to=v;
    edge[tot].next=head[u];
    head[u]=tot++;
}
void Tarjan(int u)
{
    int v;
    Low[u]=Dfn[u]=++index;
    Stack[top++]=u;
    Instack[u]=true;
    for(int i=head[u]; i!=-1; i=edge[i].next)
    {
        v=edge[i].to;
        if(!Dfn[v])
        {
            Tarjan(v);
            if(Low[u]>Low[v]) Low[u]=Low[v];
        }
        else if(Instack[v]&&Low[u]>Dfn[v]) Low[u]=Dfn[v];
    }
    if(Low[u]==Dfn[u])
    {
        scc++;
        do
        {
            v=Stack[--top];
            Instack[v]=false;
            Belong[v]=scc;
            num[scc]++;
        }
        while(v!=u);
    }
}
void solve(int n)
{
    memset(Dfn,0,sizeof(Dfn));
    memset(Low,0,sizeof(Low));
    memset(Instack,false,sizeof(Instack));
    memset(num,0,sizeof(num));
    memset(mark,0,sizeof(mark));
    index=scc=top=0;
    for(int i=1; i<=n; i++)
        if(!Dfn[i])
            Tarjan(i);
    for(int i=1; i<=n; i++)
        for(int j=head[i]; j!=-1; j=edge[j].next)
        {
            if(Belong[i]!=Belong[edge[j].to])
                mark[Belong[i]]=true;
        }
    int pos=0,lab=0;
    for(int i=1; i<=scc; i++)
        if(!mark[i])
        {
            lab++;
            pos=i;
        }
    if(lab>1)printf("0\n");
    else printf("%d\n",num[pos]);

}
int main()
{
    scanf("%d%d",&n,&m);
    tot=0;
    memset(head,-1,sizeof(head));
    for(int i=1; i<=m; i++)
    {
        scanf("%d%d",&x,&y);
        addedge(x,y);
    }
    solve(n);
}
```
