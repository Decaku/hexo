---
title: CometOJ contest#0 B
date: 2019-04-03 16:03:34
tags: 动态规划
categories: 动态规划
mathjax: true

---

# 描述:

[传送门](https://www.cometoj.com/contest/34/problem/B?problem_id=1474)

---
<!-- more -->

# 思路：

$ 发现访问到的城市都是连续的，所以可以当成区间来考虑， \\
设dp(i,x,y)表示第i天访问的城市左边有x个城市已被访问， \\
右边有y个城市已被访问对答案做出的贡献,不难想到以下的 \\
转移。 \\
dp(i,x,y)\cdot q \to dp(i+1,max(x-1,0),y+1) \\
dp(i,x,y)\cdot p \to dp(i+1,x+1,max(y-1,0) \\
dp(i,x,y)\cdot (100-p-q) \to dp(i+1,x,y) \\
要保证访问的城市能看作区间，所以要求 \\
x+y+1 \leq n-1 \\
根据100^{m-1} f(i)=\sum_{x+y+1=i} dp(m,x,y) \\
即可计算f(1)到f(n-1)中的值，再根据和为定值计算f(n) \\
这题就做完了。 $

---
```
#include<bits/stdc++.h>
using namespace std;
#define ll long long
const ll mod=1e9+7;
ll dp[505][505][505];
ll f[505];
ll n,m,k,p,q,t;
ll quick_pow(ll a,ll b)
{
    ll res=1;
    while(b)
    {
        if(b&1)
            res=res*a%mod;
        a=a*a%mod;
        b>>=1;
    }
    return res;
}
int main()
{
    scanf("%lld",&t);
    while(t--)
    {
        scanf("%lld %lld %lld %lld %lld",&n,&m,&k,&p,&q);
        for(int i=0; i<=m; i++)
        {
            for(int j=0; j<=i; j++)
            {
                for(int k=0; k<=i; k++)
                    dp[i][j][k]=0;
            }
        }
        memset(f,0,sizeof(f));
        dp[1][0][0]=1;
        for(ll i=1; i<m; i++)
        {
            for(ll x=0; x<=i; x++)
            {
                for(ll y=0; y<=i; y++)
                {
                    if(x+y+1>=n)
                        break;
                    dp[i+1][max(x-1,0ll)][y+1]+=(dp[i][x][y]*q%mod);
                    dp[i+1][max(x-1,0ll)][y+1]%=mod;

                    dp[i+1][x+1][max(y-1,0ll)]+=(dp[i][x][y]*p%mod);
                    dp[i+1][x+1][max(y-1,0ll)]%=mod;

                    dp[i+1][x][y]+=(dp[i][x][y]*(100-p-q)%mod);
                    dp[i+1][x][y]%=mod;
                }
            }
        }
        for(ll i=1; i<n; i++)
        {
            for(ll x=0; x<=i; x++)
            {
                for(ll y=0; y<=i; y++)
                {
                    if(x+y+1!=i)
                        continue;
                    f[i]+=dp[m][x][y];
                    f[i]%=mod;
                }
            }
        }
        f[n]=quick_pow(100,m-1);
        for(ll i=1; i<n; i++)
        {
            f[n]-=f[i];
            f[n]=(f[n]+mod)%mod;
        }
        ll ans=0;
        for(ll i=1; i<=n; i++)
        {
            ans+=(quick_pow(i,k)*f[i])%mod;
            ans%=mod;
        }
        printf("%lld\n",ans);
    }

}
```
