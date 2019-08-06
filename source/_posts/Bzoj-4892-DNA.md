---
title: Bzoj 4892 DNA
date: 2019-04-15 23:17:01
tags: 后缀数组
categories: 后缀数组
---

# 描述:

[传送门](https://www.lydsy.com/JudgeOnline/problem.php?id=4892)

---
<!-- more -->

# 思路：

$在第一个串里枚举每个位置,能匹配就向后跳lcp个单位，不能 \\
匹配就向后跳一个单位并记录一下不匹配的次数，因为不匹配 \\
次数是3就可以break了，所以复杂度十分优秀，lcp的话把两个 \\
串拼接起来求SA然后rmq预处理以后可以O1查询。$

---

```
/**************************************************************
    Problem: 4892
    User: Decaku
    Language: C++
    Result: Accepted
    Time:7540 ms
    Memory:22788 kb
****************************************************************/
 
#include<bits/stdc++.h>
using namespace std;
const int maxn=2e5+100;
int n,m;
int x[maxn],y[maxn],t[maxn],sa[maxn],rk[maxn],hg[20][maxn],lg[maxn];
char s[maxn],s1[maxn];
int a[maxn];
 
bool cmp(int i,int j,int k)
{
    return y[i]==y[j]&&y[i+k]==y[j+k];
}
 
void get_sa()
{
    for(int i=0;i<=m;i++) t[i]=0; //多样例注意清空
    for(int i=1;i<=n;i++) t[x[i]=a[i]]++;
    for(int i=1;i<=m;i++) t[i]+=t[i-1];
    for(int i=n;i>=1;i--) sa[t[x[i]]--]=i;
    for(int k=1;k<=n;k<<=1)
    {
        int p=0;
        for(int i=0;i<=m;i++)
            y[i]=0;
        for(int i=n-k+1;i<=n;i++)
            y[++p]=i;
        for(int i=1;i<=n;i++)
            if(sa[i]>k) y[++p]=sa[i]-k;
        for(int i=0;i<=m;i++) t[i]=0;
        for(int i=1;i<=n;i++) t[x[y[i]]]++;
        for(int i=1;i<=m;i++) t[i]+=t[i-1];
        for(int i=n;i>=1;i--) sa[t[x[y[i]]]--]=y[i];
        swap(x,y);
        x[sa[1]]=p=1;
        for(int i=2;i<=n;i++) x[sa[i]]=cmp(sa[i],sa[i-1],k)?p:++p;
        if(p>=n) break;
        m=p;
    }
}
void get_rk()
{
    for(int i=1;i<=n;i++) rk[sa[i]]=i;
}
 
void get_height()
{
    for(int i=2;i<maxn;i++) lg[i]=lg[i>>1]+1;
 
    for(int i=1,j=0;i<=n;i++)
    {
        if(j) --j;
        while(a[i+j]==a[sa[rk[i]-1]+j]) ++j;
        hg[0][rk[i]]=j;
    }
    for(int j=1;j<=lg[n];++j)
        for(int i=1;i+(1<<j)-1<=n;i++)
        hg[j][i]=min(hg[j-1][i],hg[j-1][i+(1<<(j-1))]);
}
 
int lcp(int i,int j)
{
    i=rk[i];j=rk[j];if(i>j) swap(i,j);
    if(i==j) return 1e9;++i;
    int l=lg[j-i+1];
    return min(hg[l][i],hg[l][j-(1<<l)+1]);
}
 
int main()
{
    int T;
    scanf("%d",&T);
    while(T--)
    {
        scanf("%s",s+1);
        scanf("%s",s1+1);
        int lens=strlen(s+1);
        int lent=strlen(s1+1);
        for(int i=1;i<=lens;i++) a[i]=s[i];
        for(int i=1;i<=lent;i++) a[lens+i]=s1[i];
        m=300;
        n=lens+lent;
        get_sa();
        get_rk();
        get_height();
        int ans=0;
        for(int i=1;i<=lens-lent+1;i++)
        {
            int tt=0;
            for(int j=1;j<=lent&&tt<=3;)
            {
                if(a[i+j-1]!=a[lens+j]) tt++,j++;
                else j+=lcp(i+j-1,lens+j);
            }
            ans+=(tt<=3);
        }
        printf("%d\n",ans);
    }
}
```