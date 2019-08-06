---
title: 'Bzoj4919 [Lydsy1706月赛]大根堆'
date: 2019-02-20 13:47:25
tags: [启发式合并, 最长上升子序列]
categories: 启发式合并
mathjax: ture

---

# 描述:

[传送门](https://www.lydsy.com/JudgeOnline/problem.php?id=4919)

---
<!-- more -->

# 思路:

$本质上就是求树上最长上升子序列,联想到序列LIS的nlogn做法, \\
但是对于子树要进行合并,考虑到集合具有有序性,使用多重 \\
集合来维护每个结点,在dfs回溯时合并子树,并采用启发式 \\
合并来降低复杂度,总复杂度为nlognlogn。 $

---
```
/**************************************************************
    Problem: 4919
    User: Decaku
    Language: C++
    Result: Accepted
    Time:864 ms
    Memory:22472 kb
****************************************************************/
 
#include<bits/stdc++.h>
using namespace std;
const int maxn=2e5+5;
int n,w[maxn],head[maxn],cnt;
multiset<int>m[maxn];
 
struct Node
{
    int nex,to;
} e[maxn<<1];
 
void ini()
{
    memset(head,-1,sizeof(head));
    cnt=-1;
}
void add_edge(int u,int v)
{
    e[++cnt].nex=head[u];
    e[cnt].to=v;
    head[u]=cnt;
 
    e[++cnt].nex=head[v];
    e[cnt].to=u;
    head[v]=cnt;
}
 
void Merge(int a,int b)
{
    if(m[a].size()<m[b].size())
        swap(m[a],m[b]);
    multiset<int>::iterator it=m[b].begin();
    while(it!=m[b].end())
    {
        m[a].insert(*it);
        it++;
    }
}
 
void dfs(int u,int f)
{
    for(int i=head[u]; i!=-1; i=e[i].nex)
    {
        int v=e[i].to;
        if(v==f)
            continue;
        dfs(v,u);
        Merge(u,v);
    }
    multiset<int>::iterator it;
 
    it=m[u].lower_bound(w[u]);
    if(it!=m[u].end())
        m[u].erase(it);
    m[u].insert(w[u]);
 
}
 
int main()
{
    scanf("%d",&n);
    ini();
    for(int i=1; i<=n; i++)
    {
        int v;
        scanf("%d %d",&w[i],&v);
        add_edge(i,v);
    }
    dfs(1,0);
    printf("%d\n",m[1].size());
}
```