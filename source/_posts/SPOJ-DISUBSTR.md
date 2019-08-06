---
title: SPOJ - DISUBSTR
date: 2019-02-05 16:05:54
tags: 后缀数组
categories: 后缀数组
mathjax: true

---
# 描述:

求字符串中本质不同的子串个数。

---
<!-- more -->
# 思路:

$枚举每个排名的后缀对答案的贡献,排名为i的后缀里有n-sa[i]+1个前 \\
缀,其中前缀被重复算贡献的次数是LCP(sa[i],sa[i-1]),所以答案是 \\
\sum n-sa[i]+1-height[i]$

---
```
#include<bits/stdc++.h>
using namespace std;
const int maxn=2e3+5;
int n,m;
int x[maxn],y[maxn],t[maxn],sa[maxn],rk[maxn],height[maxn];
char s[maxn];
int a[maxn];
bool cmp(int i,int j,int k)
{
    return y[i]==y[j]&&y[i+k]==y[j+k];
}
void get_sa()
{
    for(int i=1; i<=n; ++i)
        t[x[i]=a[i]]++;
    for(int i=1; i<=m; ++i)
        t[i]+=t[i-1];
    for(int i=n; i>=1; --i)
        sa[t[x[i]]--]=i;
    for(int k=1; k<=n; k<<=1)
    {
        int p=0;
        for(int i=0; i<=m; ++i)
            y[i]=0;
        for(int i=n-k+1; i<=n; ++i)
            y[++p]=i;
        for(int i=1; i<=n; ++i)
            if(sa[i]>k)
                y[++p]=sa[i]-k;
        for(int i=0; i<=m; ++i)
            t[i]=0;
        for(int i=1; i<=n; ++i)
            t[x[y[i]]]++;
        for(int i=1; i<=m; ++i)
            t[i]+=t[i-1];
        for(int i=n; i>=1; --i)
            sa[t[x[y[i]]]--]=y[i];
        swap(x,y);
        x[sa[1]]=p=1;
        for(int i=2; i<=n; ++i)
            x[sa[i]]=cmp(sa[i],sa[i-1],k)?p:++p;
        if(p>=n)
            break;
        m=p;
    }
}

void get_rk()
{
    for(int i=1; i<=n; ++i)
        rk[sa[i]]=i;
}
void get_height()
{
    for(int i=1,j=0; i<=n; ++i)
    {
        if(j)
            j--;
        while(a[i+j]==a[sa[rk[i]-1]+j])
            ++j;
        height[rk[i]]=j;
    }
}
void ini()
{
    memset(sa,0,sizeof(sa));
    memset(rk,0,sizeof(rk));
    memset(height,0,sizeof(height));
    memset(x,0,sizeof(x));
    memset(y,0,sizeof(y));
    memset(t,0,sizeof(t));
    memset(a,0,sizeof(a));
}
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        ini();
        scanf("%s",s+1);
        n=strlen(s+1);
        m=300;
        for(int i=1; i<=n; i++)
            a[i]=s[i];
        get_sa();
        get_rk();
        get_height();
        int ans=0;
        for(int i=1; i<=n; i++)
        {
            ans+=(n-sa[i]+1-height[i]);
        }
        printf("%d\n",ans);
    }
}
```
