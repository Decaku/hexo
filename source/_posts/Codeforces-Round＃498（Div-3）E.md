---
title: Codeforces Round＃498 E
date: 2019-02-05 00:44:37
tags: dfs
categories: dfs
mathjax: true

---
# 描述:

$给出一个有根树，多组询问，求编号为u的结点的dfs序的第k个儿子。$

---
<!-- more -->
# 思路:

$dfs预处理树上一些信息乱搞就行了。$

---
```
#include<cstdio>
#include<algorithm>
#include<vector>
#include<iostream>
#include<queue>
#include<cstring>
using namespace std;

const int maxn = 2e5+10;
queue<int>que[maxn];
int a[maxn],b[maxn],sum[maxn];////a存dfs序号,b存序号对应的结点编号
int cnt1=1;
void dfs(int k)
{
    int cnt2=0;
    while(que[k].empty()==false)
    {
        int t=que[k].front();
        cnt2++;
        que[k].pop();
        cnt1++;
        a[t]=cnt1;
        b[cnt1]=t;
        dfs(t);
        cnt2+=sum[t];
    }
    sum[k]+=cnt2;
    return;
}

int main()
{
    memset(sum,0,sizeof(sum));
    int n,q,tem,x,y;
    cin>>n>>q;
    for(int i=1; i<n; i++)
    {
        cin>>tem;
        que[tem].push(i+1);
    }
    dfs(1);
    a[1]=1;
    b[1]=1;
    while(q--)
    {
        cin>>x>>y;
        if(sum[x]+1<y)
            cout<<"-1"<<endl;
        else cout<<b[a[x]+y-1]<<endl;
    }
    return 0;
}
```
