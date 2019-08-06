---
title: Zoj3261 Connections in Galaxy War
date: 2019-02-04 23:13:06
tags: [并查集, 删边, 离线]
categories: 并查集
mathjax: true

---
# 描述:

$给n个点和m个边，点带权。然后给若干个操作，操作一是删边, \\
操作二是询问这个点所在连通分量的代表点的编号。所以一个连 \\
通分量里的代表点，它的权必须是最大的。$

---
<!-- more -->
# 思路:

$ 权最大的是代表点，所以在合并连通分量时以权大的为父亲, \\
然后我们离线处理每个询问,用map维护。$

---
```
#include<stdio.h>
#include<stack>
#include<map>
#include<iostream>
using namespace std;


int p[10005],a[10005];
struct query
{
    char str[10];
    int p1,p2;////有2个参数

} tem;

struct edge
{
    int u,v;
    bool operator < (const edge &a)const
    {
        if(u==a.u)
            return v<a.v;
        else return u<a.u;
    }

} e[20005],teme;


int findset(int x)
{
    return x==p[x]?p[x]:p[x]=findset(p[x]);

}
void join(int x,int y)
{
    int px=findset(x),py=findset(y);
    if(px!=py&&a[px]>=a[py])
        p[py]=px;
    if(px!=py&&a[px]<a[py])
        p[px]=py;
}


int main()
{
    int n,m,aa,bb,q;
    bool flag=false;
    while(~scanf("%d",&n))
    {
        if(flag)
            cout<<endl;
        flag=true;
        stack<int>ans;
        stack<query>st;
        map<edge,bool>mp;
        for(int i=0; i<n; i++)
        {
            scanf("%d",&a[i]);
            p[i]=i;
        }
        scanf("%d",&m);
        for(int i=0; i<m; i++)
        {
            scanf("%d%d",&aa,&bb);
            e[i].u=min(aa,bb);
            e[i].v=max(aa,bb);
            mp[e[i]]=true;
        }

        scanf("%d",&q);
        for(int i=0; i<q; i++)
        {
            scanf("%s%d",&tem.str,&tem.p1);
            if(tem.str[0]=='d')
            {
                scanf("%d",&tem.p2);
                teme.u=min(tem.p1,tem.p2),teme.v=max(tem.p1,tem.p2);
                mp[teme]=false;
                st.push(tem);
            }
            else tem.p2=-1,st.push(tem);
        }

        map<edge, bool>::iterator iter=mp.begin();

        while(iter!=mp.end())
        {
            if(iter->second)
            {
                join(iter->first.u,iter->first.v);
            }
            iter++;
        }

        while(!st.empty())
        {
            tem=st.top();
            st.pop();
            if(tem.str[0]=='q')
            {
                if(a[findset(tem.p1)]==a[tem.p1]) ans.push(-1);
                else ans.push(findset(tem.p1));
            }
            else join(tem.p1,tem.p2);
        }
        while(!ans.empty())
        {
            printf("%d\n",ans.top());
            ans.pop();
        }
    }
    return 0;
}
```