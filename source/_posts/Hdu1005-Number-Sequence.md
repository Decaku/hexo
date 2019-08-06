---
title: Hdu1005 Number Sequence
date: 2019-02-05 00:23:03
tags: 找规律
categories: 数论
mathjax: true

---
# 描述:

$ f(1)=1, f(2)=1, f(n) = (A \* f(n - 1) + B \* f(n - 2)) \mod 7. \\
然后计算f(n),n很大。$

---
<!-- more -->

找循环节，循环节长度最大是49，但是发现前几项可能不出现在循环节里，所以舍弃前50项即可。

---
```
#include<cstdio>
#include<iostream>
#include<algorithm>
#include<vector>
#include<set>
#include<cmath>
#include<queue>
#include<vector>
using namespace std;
int x,y,n,tem;
int ans[100];
int main()
{
    while(cin>>x>>y>>n&&(x+y+n))
    {
        ans[1]=ans[2]=1;
        for(int i=3; i<=100; i++)
            ans[i]=(ans[i-1]*x+ans[i-2]*y)%7;
       int a=ans[50];
       int b=ans[51];
        for(int i=52; i<=99; i++)
            if(ans[i]==a&&ans[i+1]==b)
            {
                tem=i;
                break;
            }
        if(n<50) cout<<ans[n]<<endl;
       else cout<<ans[(n-49)%(tem-50)+49]<<endl;
    }
    return 0;
}
```
