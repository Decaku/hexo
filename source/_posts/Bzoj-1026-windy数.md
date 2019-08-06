---
title: Bzoj 1026 windy数
date: 2019-02-05 04:30:18
tags: 数位dp
categories: 动态规划
mathjax: true

---
# 描述:

求区间里满足相邻数位差值的绝对值不小于2的数的个数。

---
<!-- more -->
# 思路:

$数位dp,对有无前导0分情况讨论。$

---
```
/**************************************************************
    Problem: 1026
    User: Decaku
    Language: C++
    Result: Accepted
    Time:48 ms
    Memory:1288 kb
****************************************************************/
 
#include<bits/stdc++.h>
using namespace std;
int dp[15][15];
void init()
{
    for(int i=0; i<=9; i++)
        dp[1][i]=1;
    for(int i=2; i<=10; i++)
        for(int j=0; j<=9; j++)
            for(int k=0; k<=9; k++)
            {
                if(abs(j-k)>=2)
                    dp[i][j]+=dp[i-1][k];
            }
}
 
int query(int x)
{
    int d[15]= {},cnt=0,ret=0;
    while(x)
    {
        d[++cnt]=x%10;
        x/=10;
    }
    for(int i=1; i<cnt; i++)
        for(int j=1; j<10; j++)
            ret+=dp[i][j];
    for(int i=1; i<d[cnt]; i++)
        ret+=dp[cnt][i];
    for(int i=cnt-1; i>=1; i--)
    {
        for(int j=0; j<d[i]; j++)
        {
            if(abs(j-d[i+1])>=2)
                ret+=dp[i][j];
        }
        if(abs(d[i]-d[i+1])<2)  break;
    }
    return ret;
}
 
int main()
{
    init();
    int a,b;
    scanf("%d %d",&a,&b);
    printf("%d\n",query(b+1)-query(a));
}
```