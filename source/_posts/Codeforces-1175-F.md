---
title: Codeforces 1175 F
date: 2019-06-09 01:37:23
tags: st表
categories: st表
---


# 描述:

[传送门](http://codeforces.com/contest/1175/problem/F)

$统计有多少区间[l,r]满足区间是1到r-l+1的排列。$

---
<!-- more -->

#思路：

$对每个点i预处理r[i]，表示当[i,r[i]]这段区间的每一个前缀区间 \\
里的数都是两两不重复的,这件事可以O(n)做，然后统计答案时对每个 \\
点i都统计[i,r[i]]这段区间的前缀[i,j]，判断[i,j]区间最大值mx \\
是否等于j-i+1即可,发现区间不成立时，我们可以让j=i+mx-1加速j \\
的枚举，可以证明合法区间不会太多,所以这个算法的复杂度可以证 \\
明是O(nlogn)。 $

---

```

#include<bits/stdc++.h>
using namespace std;
const int maxn=3e5+5;
#define ll long long
int n,a[maxn],r[maxn],pos[maxn];
int mm[maxn],rmq[maxn][20];

void ini()
{
    mm[0]=-1;
    for(int i=1;i<maxn;i++)
        mm[i]=((i&i-1)==0)?mm[i-1]+1:mm[i-1];
    for(int i=1;i<=n;i++) rmq[i][0]=a[i];
    for(int j=1;j<=mm[n];j++)
        for(int i=1;i+(1<<j)-1<=n;i++)
        rmq[i][j]=max(rmq[i][j-1],rmq[i+(1<<(j-1))][j-1]);
}

int query(int l,int r)
{
    int k=mm[r-l+1];
    return max(rmq[l][k],rmq[r-(1<<k)+1][k]);
}
int main()
{
    scanf("%d",&n);
    for(int i=1;i<=n;i++)
        scanf("%d",&a[i]);
    r[n]=n;
    pos[a[n]]=n;
    for(int i=n-1;i>=1;i--)
    {
        if(pos[a[i]])
        {
            r[i]=min(pos[a[i]]-1,r[i+1]);
        }
        else
        {
            r[i]=r[i+1];
        }
        pos[a[i]]=i;
    }
    ll ans=0;
    //for(int i=1;i<=n;i++)
      //  cout<<r[i]<<" ";
    //return 0;
    ini();
    for(int i=1;i<=n;i++)
    {
        for(int j=i;j<=r[i];)
        {
            if(query(i,j)==j-i+1)
                ans++,j++;
            else
            {
                int mx=query(i,j);
                j=i+mx-1;
            }
        }
    }
    cout<<ans<<endl;
}

```
