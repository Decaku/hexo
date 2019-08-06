---
title: 牛客练习赛22-C简单瞎搞题 bitset+dp
date: 2019-02-04 15:52:02
tags: [bitset, dp]
categories: 动态规划
---
# 描述:

给出$n$个区间，然后分别给出区间的左右边界，可以在每个区间中选择一个整数，问这$n$个整数的平方和有多少种不同的取值。

---
<!-- more -->
# 思路:

$ f_i $表示一个bitset，bitset里有几个1，就表示这i个区间所能得到的平方和的取值种数。

转移方程为 *f[i]=f[i] | f[i-1]<<k^k (k from l to r)*

状态转移的复杂度是n，每次操作对应的复杂度是l-r。

---
```
#include <iostream>
#include<cstdio>
#include<stdio.h>
#include<bitset>

using namespace std;

bitset<1000000+10>f[105];

int main()
{
    int n,l,r;
    cin>>n;
    f[0][0]=1;
    for (int i=1; i<=n; i++)
    {
        scanf("%d %d",&l, &r);
        for(int j=l; j<=r; j++)
        {
            f[i]=f[i]|f[i-1]<<j*j;
        }
    }
    printf("%d\n", f[n].count());

    return 0;
}
```
