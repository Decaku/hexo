---
title: Hdu6432 Cyclic
date: 2019-02-04 21:22:47
tags: 容斥定理
categories: 组合数学
mathjax: true
---
# 描述:

$1-n的n个数组成一个环,要求i后面不能放i+1，n后面不能放1 \\
经旋转能得到的2种视为同一种方案,求所有本质不同的方案数。$

---
<!-- more -->
# 思路:

容斥计数。

---
```
#include <string>
#include <iostream>
#include <algorithm>
#include<cmath>
#include<iomanip>
using namespace std;
typedef long long ll;
const int mod=998244353;
const int maxn=1e5+5;
ll ans[maxn];

int main()
{
    int t,n;
    cin>>t;
    ans[0]=1;
    ans[1]=0;
    ans[2]=0;
    ans[3]=1;
    ans[4]=1;
    for(int i=5; i<maxn; i++)
    {
        ans[i]=((i-3)*ans[i-1]%mod+(i-2)*(2*ans[i-2]+ans[i-3])%mod)%mod;

    }
    while(t--)
    {
        cin>>n;
        cout<<ans[n]<<endl;
    }
    return 0;
}
```