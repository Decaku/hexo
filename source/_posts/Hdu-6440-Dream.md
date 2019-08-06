---
title: Hdu 6440 Dream
date: 2019-02-04 23:55:13
tags: 费马小定理
categories: 数论
mathjax: true

---
# 描述:
 
$重新定义加法和乘法，使得对于p是素数时(a+b)^{p}=a^{p}+b^{p}。$

---
<!-- more -->
# 思路:

$根据费马小定理有x^{p-1}与1在模p的意义下同余(p是素数)。 \\
 于是做如下定义：a+b=(a+b) \mod p \\
 ab=ab \mod p \\
 易证 (a+b)^{p}=a^{p}+b^{p}=(a+b) \mod p $

---
```
include<cstdio>
#include<iostream>
#include<algorithm>
#include<vector>
#include<set>
#include<queue>
using namespace std;

int t,p;
int main()
{
    scanf("%d",&t);
    while(t--)
    {
        scanf("%d",&p);
        for(int i=1; i<=p; i++)
        {
            for(int j=1; j<=p; j++)
            {
                printf("%d ",((i-1)%p+(j-1)%p)%p);
            }
            printf("\n");
        }
        for(int i=1; i<=p; i++)
        {
            for(int j=1; j<=p; j++)
            {
                printf("%d ",((i-1)%p*(j-1)%p)%p);
            }
            printf("\n");
        }
    }
    return 0;
}
```