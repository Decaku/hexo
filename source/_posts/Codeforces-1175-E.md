---
title: Codeforces 1175 E
date: 2019-06-09 01:30:16
tags: 倍增
categories: 倍增
---


# 描述:

[传送门](http://codeforces.com/contest/1175/problem/E)

$给出许多线段，然后若干询问，每次询问覆盖一个区间至少要几条线段$

---
<!-- more -->

#思路：

$首先考虑若有多个线段都能覆盖同一点时我们一定选择右端点最 \\
远的，所以在每个点的决策都是固定的，所以直接倍增预处理 \\
f[i][j]代表在i点跳2^{j}个线段能到的最右边的点，回答就是log 。$

---

```

#include<bits/stdc++.h>
using namespace std;

const int maxn=2e5+5;
const int mx=5e5+5;
int n,m,nex[mx][22];
vector<int>v[mx];

int main()
{
    scanf("%d %d",&n,&m);
    for(int i=1; i<=n; i++)
    {
        int l,r;
        scanf("%d %d",&l,&r);
        v[l].push_back(r);
    }
    int tem=-1;
    for(int i=0; i<mx; i++)
    {
        for(int j=0; j<v[i].size(); j++)
        {
            tem=max(tem,v[i][j]);
        }
        if(tem>i)
            nex[i][0]=tem;
        else
            nex[i][0]=i;
    }
    for(int i=mx; i>=0; i--)
    {
        for(int j=1; j<22; j++)
            nex[i][j]=nex[nex[i][j-1]][j-1];
    }

    while(m--)
    {
        int l,r,ans=1;
        scanf("%d %d",&l,&r);
        int p=l;
        if(nex[p][20]<r)
            ans=-1;
        else
        {
            for(int j=20; j>=0; j--)
            {
                if(nex[p][j]<r)
                    ans+=(1<<j),p=nex[p][j];

            }
        }
        printf("%d\n",ans);
    }
}

```
