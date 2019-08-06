---
title: 2018ICPC南京网络赛 AC Challenge 状压dp
date: 2019-02-04 16:21:13
tags: 状压dp
categories: 动态规划
mathjax: true

---
# 描述:

$n个题，做每个题之前必须把限制的某几个题做完。\\
做第i个题的得分是a*w_i+b,w_i是第i题权重，a,b$都是常数，问能得到的最高分。

---
<!-- more -->
# 思路:

考虑到$n$最大只有20，且每个题只有做和不做两种状态，所以可以状压dp。

对于每个状态的枚举时，首先判断这种状态是否符合条件。如果这种状态是符合条件的，那考虑这种状态中所有已经做了的题，分别把它们状态设为没做，这样就找到所有能转移到目前状态的前驱状态。

---
```
#include<bits/stdc++.h>
using namespace std;
int t;
struct Node
{
    int a,b,s;
} node[25];
vector<int>vec[25];
int dp[(1<<20)+1];

int main()
{
    while(cin>>t)
    {
        for(int i=1; i<=t; i++)
        {
            cin>>node[i].a>>node[i].b>>node[i].s;
            int tem;
            for(int j=1; j<=node[i].s; j++)
            {
                cin>>tem;
                vec[i].push_back(tem);
            }
        }
        memset(dp,0,sizeof(dp));
        int ans=0;
        for(int i=0; i<(1<<t); i++)
        {
            bool ok=true;
            for(int j=1; j<=t; j++)
            {
                if(!(i&(1<<(j-1)))) continue;
                for(int k=0;k<vec[j].size();k++)
                {
                    int tmp=vec[j][k];
                    if(!(i&(1<<(tmp-1))))
                    {
                        ok=false;
                        break;
                    }
                }
                if(!ok) break;
            }
            if(!ok) continue;//如果不满足大条件 就丢掉
            for(int j=1;j<=t;j++)
            {
                if(!(i&(1<<(j-1)))) continue;//这题做过的才能继续这个算法
                int s=i,cnt=0;//统计i的二进制里有几个1，也就是有几题做过
                while(s)
                {
                    if(s&1) cnt++;
                    s/=2;
                }
                dp[i]=max(dp[i],dp[i^(1<<(j-1))]+cnt*node[j].a+node[j].b);//让做过的这题没做就是前一个状态
            }
            ans=max(ans,dp[i]);
        }
        cout<<ans<<endl;
    }
    return 0;
}
```

