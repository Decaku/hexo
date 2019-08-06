---
title: 'Codeforces #511 div2 D. Bicolorings'
date: 2019-02-05 00:36:40
tags: dp
categories: 动态规划
mathjax: true

---
# 描述:

$ 有个2行n列的格子图,用2种颜色给格子涂色,相邻格子颜色 \\
不能相同,求不同的染色方案数。$

---
<!-- more -->
# 思路:

$类似状压dp的思路，只有两行，所以每一列只有4个状态, \\
所以可以考虑枚举每一列，这样复杂度是O(n^4)，n只有1000, \\
题目又要求把方格分成k个连通块，我们在dp中再加一维即可, \\
k最大是2n的，所以最多做8 \* (10)^6次循环。$

---
```
#include<bits/stdc++.h>
using namespace std;
const int mod=998244353;
long long dp[5][1100][2100];
int main()
{
    int n,k;
    cin>>n>>k;
    memset(dp,0,sizeof(dp));
    dp[0][1][1]=1;
    dp[1][1][1]=0;
    dp[2][1][1]=0;
    dp[3][1][1]=1;

    dp[0][1][2]=0;
    dp[1][1][2]=1;
    dp[2][1][2]=1;
    dp[3][1][2]=0;


    for(int j=2; j<=n; j++)
        for(int i=0; i<=3; i++)
            for(int t=1; t<=k; t++)
            {
                if(i==0)
                    dp[i][j][t]=(dp[0][j-1][t]+dp[1][j-1][t]+dp[2][j-1][t]+dp[3][j-1][t-1])%mod;
                else if(i==1)
                    dp[i][j][t]=(dp[0][j-1][t-1]+dp[1][j-1][t]+dp[2][j-1][t-2]+dp[3][j-1][t-1])%mod;
                else if(i==2)
                    dp[i][j][t]=(dp[0][j-1][t-1]+dp[1][j-1][t-2]+dp[2][j-1][t]+dp[3][j-1][t-1])%mod;
                else
                    dp[i][j][t]=(dp[0][j-1][t-1]+dp[1][j-1][t]+dp[2][j-1][t]+dp[3][j-1][t])%mod;

            }

    cout<<(dp[0][n][k]+dp[1][n][k]+dp[2][n][k]+dp[3][n][k])%mod<<endl;
}
```
