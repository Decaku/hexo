---
title: Hdu 6447 YJJ Salesman
date: 2019-02-04 21:43:32
tags: [树状数组,dp,离散化]
categories: 动态规划
mathjax: true

---
# 描述:

有一个矩阵，某些点有宝物，宝物有权重，某人从左下角出发，只能向右，上，右上走，求取到的物品权重之和最大值。

---
<!-- more -->
# 思路:

$首先离散化地图,然后推出朴素的转移方程 \\
dp[i][j]=max\{dp[i-1][j],dp[i][j-1],dp[i-1][j-1]+w\}, \\
N^{2}复杂度对(10)^{5}数据会T。考虑加速dp。先把有宝物  \\
的地点排序,按照从左向右，从上到下的顺序排序。考虑在 \\
(x,y)处获得的最大收益，其实就是要求在(x-1,y-1)这个 \\
矩形内的最大收益加上w(x,y)。这是一个区间最值问题, \\
还涉及到点修改，可以用树状数组维护。$

---
```
#include<cstdio>
#include<bits/stdc++.h>
using namespace std;
const int maxn=1e5+5;

int t,n,tree[maxn],yy[maxn];

struct Node
{
    int x,y,z;
} node[maxn];
bool cmp(Node a,Node b)
{
    if(a.x==b.x)
        return a.y>b.y;
    return a.x<b.x;
}

int lowbit(int x)
{
    return x&(-x);
}

void uppdate(int x,int tem)
{
    for(int i=x; i<=maxn; i+=lowbit(i))
    {
        tree[i]=max(tree[i],tem);
    }

}

int query(int x)
{
    int ret=0;
    for(int i=x; i>=1; i-=lowbit(i))
    {
        ret=max(ret,tree[i]);
    }
    return ret;
}

int main()
{
    scanf("%d",&t);
    while(t--)
    {
        scanf("%d",&n);
        memset(tree,0,sizeof(tree));
        for(int i=1; i<=n; i++)
        {
            scanf("%d%d%d",&node[i].x,&node[i].y,&node[i].z);
            yy[i]=node[i].y;
        }
        sort(yy+1,yy+n+1);
        int Size=unique(yy+1,yy+n+1)-yy-1;
        for(int i=1; i<=n; i++)
            node[i].y=lower_bound(yy+1,yy+1+Size,node[i].y)-yy;
        sort(node+1,node+n+1,cmp);
        int ans=0;
        for(int i=1; i<=n; i++)
        {
            int val=query(node[i].y-1)+node[i].z;
            ans=max(ans,val);
            uppdate(node[i].y,val);
        }
        cout<<ans<<endl;
    }
    return 0;
}
```

