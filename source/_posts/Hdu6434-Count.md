---
title: Hdu6434 Count
date: 2019-02-04 21:32:01
tags: 数论
categories: 数论
mathjax: true
---
# 描述:

$求\sum _{i=1}^n \sum _{j=1}^{i-1} [gcd(i+j,i-j)=1] $

---
<!-- more -->
# 思路:

$先做变量代换，令a=i-j,先枚举i,再枚举a,然后对等式变形, \\
题意转化为对于每个i，求有多少个小于它的a满足gcd(i,a)=1 \\
且a是奇数,当i是奇数时,答案为\varphi(i)/2,当i是偶数时, \\
答案是 \varphi(i),注意特判i=1时,答案为0。$

---
```
#include <cstdio>
#include <cstring>
#include <string>
#include <iostream>
#include <algorithm>
#include<cmath>
#include<iomanip>
using namespace std;
typedef long long ll;
const int maxn=2e7+5;
int phi[maxn];
ll sum[maxn];
void phi_table()
{
    memset(phi,0,sizeof(phi));
    phi[1]=1;
    for(int i=2; i<maxn; i++)if(!phi[i])
            for(int j=i; j<maxn; j+=i)
            {
                if(!phi[j]) phi[j]=j;
                phi[j]=phi[j]/i*(i-1);
            }
}
int main()
{
    phi_table();
    sum[1]=0;
    sum[2]=phi[2];
    for(int i=3; i<maxn; i++)
        if(i%2==1) sum[i]=sum[i-1]+phi[i]/2;
        else sum[i]=sum[i-1]+phi[i];
    int t,n;
    cin>>t;
    while(t--)
    {
        cin>>n;
        cout<<sum[n]<<endl;
    }
    return 0;
}
```




