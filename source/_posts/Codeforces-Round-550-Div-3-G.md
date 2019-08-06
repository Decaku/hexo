---
layout: w
title: Codeforces 550 Div.3 G
date: 2019-04-03 00:01:28
tags: 贪心
categories: codeforces
---

# 描述:

[传送门](http://codeforces.com/contest/1144/problem/G)

---
<!-- more -->

# 思路:

官方标算是动规，但是有个比较简便的贪心的做法，分别维护一个递增和一个
递减序列，然后枚举第i个元素，如果都能往两个序列里加的话，只要和第i+1
个元素比较进行贪心就行,证明比较明显，具体看代码。

---
```
#include<bits/stdc++.h>
using namespace std;
const int maxn=2e5+5;
const int inf=1e9;
int n,a[maxn],vis[maxn];
vector<int>p,q;
int main()
{
    scanf("%d",&n);
    for(int i=1; i<=n; i++)
        scanf("%d",&a[i]);
    p.push_back(inf);
    q.push_back(-1);
    for(int i=1; i<=n; i++)
    {
        int x=p.back(),y=q.back();
        if(a[i]>=x&&a[i]<=y)
        {
            puts("NO");
            return 0;
        }
        else if(a[i]<x&&a[i]>y)
        {
            if(i==n)
                vis[i]=1;
            else
            {
                if(a[i]>a[i+1])
                {
                    p.push_back(a[i]);
                    vis[i]=1;
                }
                else
                    q.push_back(a[i]);
            }
        }
        else if(a[i]<x)
        {
            p.push_back(a[i]);
            vis[i]=1;
        }
        else
            q.push_back(a[i]);
    }
    puts("YES");
    for(int i=1; i<=n; i++)
        printf("%d ",vis[i]);
    printf("\n");
}
```

