---
title: Poj2796 Feel Good
date: 2019-02-05 02:00:47
tags: 单调栈
categories: 单调栈
mathjax: true

---
# 描述:

$一串数字序列,定义value为某一区间的最小值乘该区间的和, \\
再除以区间长度,求最大的value。 $

---
<!-- more -->
# 思路:

$预处理a_i为最小值时的区间,使用单调栈从前向后和 \\
从后向前扫描两次。 $

---
```
#include<cstdio>
using namespace std;
#define ll long long
#define maxn 100005
ll st[maxn],top;
ll a[maxn],sum[maxn],l[maxn],r[maxn];
int main()
{
    top=0;
    int n;
    scanf("%d",&n);
    scanf("%lld",&a[1]);
    sum[0]=0;
    sum[1]=a[1];
    for(int i=2; i<=n; i++)
    {
        scanf("%lld",&a[i]);
        sum[i]=sum[i-1]+a[i];
    }
    st[top++]=1;
    l[1]=1;
    for(int i=2; i<=n; i++)
    {
        while(top&&a[st[top-1]]>=a[i])
        {
            top--;
        }
        if(!top) l[i]=1;
        else l[i]=st[top-1]+1;
        st[top++]=i;
    }
    top=0;
    st[top++]=n;
    r[n]=n;
    for(int i=n-1; i>=1; i--)
    {
        while(top&&a[st[top-1]]>=a[i])
        {
            top--;
        }
        if(!top) r[i]=n;
        else r[i]=st[top-1]-1;
        st[top++]=i;
    }
    ll ans=-1;
    ll _l,_r;
    for(int i=1; i<=n; i++)
    {
        if((a[i])*(sum[r[i]]-sum[l[i]-1])>ans)
        {
            ans=(a[i])*(sum[r[i]]-sum[l[i]-1]);
            _l=l[i];
            _r=r[i];
        }
    }
    printf("%lld\n",ans);
    printf("%lld %lld\n",_l,_r);
}
```