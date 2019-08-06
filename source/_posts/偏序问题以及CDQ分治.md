---
layout: w
title: 偏序问题以及CDQ分治
date: 2019-03-15 15:07:43
tags: 随笔
categories: 随笔

---
##前置技能：
$归并排序，分治，树状数组，逆序对$

<!-- more -->

## 一维偏序：
$一维偏序只需排序即可解决$

---

##二维偏序：
$第一维排序，第二维使用数据结构维护，具体过程是：先对x排序， \\
然后动态的把点的第二维加入树状数组或权值线段树里，因为当枚 \\
举到i时，已经保证[1,i-1]里的点x都比i小,所以只需统计i之前的 \\
点里y比i小的点个数即可。$

$例题：Hdu1541$
```
#include<bits/stdc++.h>
using namespace std;
const int maxn=200000;
struct star
{
    int x,y;
    bool operator<(const star& t)const
    {
        if(x==t.x)
            return y<t.y;
        return x<t.x;
    }
} a[maxn];
int n,c[maxn],cnt[maxn];
int lowbit(int x)
{
    return x&(-x);
}
int sum(int x)
{
    int res=0;
    while(x>0)
    {
        res+=c[x];
        x-=lowbit(x);
    }
    return res;
}
int add(int x,int y)
{
    while(x<maxn)
    {
        c[x]+=y;
        x+=lowbit(x);
    }
}
int main()
{
    while(scanf("%d",&n)!=EOF)
    {
        memset(c,0,sizeof(c));
        memset(cnt,0,sizeof(cnt));
        for(int i=1; i<=n; i++)
        {
            scanf("%d %d",&a[i].x,&a[i].y);
            a[i].y++; //树状数组里的点都是从1开始编号
        }
        sort(a+1,a+n+1);
        for(int i=1; i<=n; i++)
        {
            int tem=sum(a[i].y);
            cnt[tem]++;
            add(a[i].y,1);
        }
        for(int i=0; i<n; i++)
        {
            printf("%d\n",cnt[i]);
        }
    }
}
```
---

##三维偏序：
$第一维排序，第二维CDQ分治，第三维使用数据结构维护。 \\
当对区间[l,r]按x排完序后，把区间分成[l,mid]和[mid+1,r] \\
两部分,此时再对两区间分别按y排序，因为此时一定能保证 \\
左区间的x是一定小于右区间,所以可以统计区间merge时对答案 \\
的贡献，第三维z仍可使用树状数组来维护。$

$例题：bzoj3262$
```
#include<algorithm>
#include<iostream>
#include<cstring>
#include<cstdio>
#include<cmath>
using namespace std;
#define N 200005

int n,k,acnt,bcnt,pa,pb,tot;
struct hp{int x,y,z,id,ans;}f[N],a[N],b[N];
int C[N],cnt[N],ch[N];

int cmp1(hp a,hp b)
{
    return a.x<b.x||(a.x==b.x&&a.y<b.y)||(a.x==b.x&&a.y==b.y&&a.z<b.z);
}
int cmp2(hp a,hp b)
{
    return a.y<b.y;
}
void add(int loc,int val)
{
    if (!loc) return;
    for (int i=loc;i<=k;i+=i&(-i))
        C[i]+=val;
}
int query(int loc)
{
    int ans=0;
    for (int i=loc;i>=1;i-=i&(-i))
        ans+=C[i];
    return ans;
}
void cdq(int l,int r)
{
    if (l>=r) return;
    int mid=(l+r)>>1;
    cdq(l,mid);

    acnt=0;
    for (int i=l;i<=mid;++i) a[++acnt]=f[i];
    sort(a+1,a+acnt+1,cmp2);
    bcnt=0;
    for (int i=mid+1;i<=r;++i) b[++bcnt]=f[i];
    sort(b+1,b+bcnt+1,cmp2);
    pa=pb=1;tot=0;
    while (pb<=bcnt)
    {
        while (pa<=acnt&&a[pa].y<=b[pb].y)
        {
            add(a[pa].z,1);
            ch[++tot]=a[pa].z;
            ++pa;
        }
        f[b[pb].id].ans+=query(b[pb].z);
        ++pb;
    }
    for (int i=1;i<=tot;++i)
        add(ch[i],-1);

    cdq(mid+1,r);
}
int main()
{
    scanf("%d%d",&n,&k);
    for (int i=1;i<=n;++i)
        scanf("%d%d%d",&f[i].x,&f[i].y,&f[i].z);
    sort(f+1,f+n+1,cmp1);
    for (int i=1;i<=n;++i) f[i].id=i;
    cdq(1,n);
    for (int i=n-1;i>=1;--i)
        if (f[i].x==f[i+1].x&&f[i].y==f[i+1].y&&f[i].z==f[i+1].z)
            f[i].ans=max(f[i].ans,f[i+1].ans);
    for (int i=1;i<=n;++i)
        ++cnt[f[i].ans];
    for (int i=0;i<n;++i)
        printf("%d\n",cnt[i]);
}
```

---
##小结：
$对于高维偏序问题，可以采取CDQ套CDQ的方式来解决，但当维度 \\
高于五维时，CDQ分治的效率会差于n^2暴力，此外，CDQ分治无法 \\
解决强制在线的题目，此时只能写树套树了。$


