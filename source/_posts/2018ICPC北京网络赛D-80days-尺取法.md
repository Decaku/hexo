---
title: 2018ICPC北京网络赛D 80days 尺取法
date: 2019-02-04 16:14:03
tags: 尺取法
categories: 尺取法
---
# 描述:

[传送门](http://hihocoder.com/problemset/problem/1831 "With a Title")

---
<!-- more -->
# 思路:

有环，易想到复制一份在后面，然后考虑枚举起点，尺取法双指针扫描即可，复杂度为线性。

---
```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn=1e6+5;
int t;
int n,k;
int a[maxn],b[maxn],c[maxn<<1];
int main()
{

    cin>>t;
    while(t--)
    {
        cin>>n>>k;
        for(int i=1;i<=n;i++)
            scanf("%d",&a[i]);
        for(int i=1;i<=n;i++)
            scanf("%d",&b[i]);
        for(int i=1;i<=n;i++)
        {
            c[i]=a[i]-b[i];
            c[n+i]=a[i]-b[i];
        }
        int l=1,r=1;
        ll tem=0;
        bool ok=true;
        while(l<=n&&r-l+1<=n)
        {
           if(!ok) break;
           if(tem+c[r]+k>=0)
           {
               tem+=c[r];
               r++;
           }
           else
           {
               tem+=c[r];
               r++;
               while(tem+k<0)
               {
                   tem-=c[l];
                   l++;
                   if(l>r)
                   {
                       ok=false;
                       break;
                   }
               }
           }
        }
        if(!ok||l>n) cout<<-1<<endl;
        else cout<<l<<endl;
    }
    return 0;
}
```
