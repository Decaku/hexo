---
title: 2018牛客国庆集训派对day1 J Princess Principal
date: 2019-02-04 20:13:37
tags: 线段树
categories: 线段树
---

# 描述:

给出一个括号序列，然后给出多组询问，每次判断一个区间里的括号是否是匹配的。

---
<!-- more -->
# 思路:

$对于一个括号序列，如果想让所有的括号尽可能多的匹配，每个单 \\括号应该只对应唯一的另一个单括号与它匹配。那么就可以比较加入 \\a_l之前栈里留下的括号的状态和加入a_r以后栈里留下的括号的状态, \\如果一样，相当于这段区间的括号消掉了。$

$思路二，已知括号序列的匹配方式是唯一的，如果在区间里任意取出 \\一个单括号,与它匹配的单括号也必须在这个区间里。所以先预处理与 \\每一个位置单括号匹配的另一个单括号的编号x_i。 但是这时查询一个 \\区间的复杂度仍然是线性的。 但其实只需要查询这个区间里x_i的最大值 \\和最小值即可，最大值是右端点，最小值是左端点说明这个区间匹配。$

---
```
#include<bits/stdc++.h>
using namespace std;
const int maxn=1e6+5;
int n,m,q;
int a[maxn],x[maxn];
stack<int>s;
int main()
{
    scanf("%d %d %d",&n,&m,&q);
    while(!s.empty()) s.pop();
    for(int i=1;i<=n;i++)
        scanf("%d",a+i);
    for(int i=1;i<=n;i++)
    {
        if(s.empty()||!(a[i]&1))
            s.push(i);
        else if(a[s.top()]+1==a[i])
            s.pop();
        else s.push(i);
        if(s.empty()) x[i]=0;
        else x[i]=s.top();
    }
    while(q--)
    {
     int l,r;
     scanf("%d %d",&l,&r);
     if(x[l-1]==x[r])
        printf("Yes\n");
     else printf("No\n");
    }
    return 0;
}
```
---

```
#include<bits/stdc++.h>
using namespace std;
const int maxn=1e6+5;
int n,m,q;
int a[maxn],b[maxn],x[maxn<<2],y[maxn<<2];
stack<int>s;
void build(int R,int l,int r)
{
   if(l==r)
   {
       x[R]=b[l];
       y[R]=b[l];
       return;
   }
   int mid=l+r>>1;
   build(R<<1,l,mid);
   build(R<<1|1,mid+1,r);
   x[R]=min(x[R<<1],x[R<<1|1]);
   y[R]=max(y[R<<1],y[R<<1|1]);
}
void ask(int R,int l,int r,int ql,int qr,int &u,int &v)
{
    if(ql<=l&&r<=qr)
    {
      u=min(u,x[R]);
      v=max(v,y[R]);
      return ;
    }
    int mid=l+r>>1;
    if(ql<=mid) ask(R<<1,l,mid,ql,qr,u,v);
    if(qr>mid) ask(R<<1|1,mid+1,r,ql,qr,u,v);
}
int main()
{
    scanf("%d %d %d",&n,&m,&q);
    for(int i=1;i<=n;i++)
      {
          scanf("%d",a+i);
          b[i]=-1;
          if(s.empty()||!(a[i]&1)) s.push(i);
          else if (a[s.top()]+1==a[i]) b[i]=s.top(),b[s.top()]=i,s.pop();
          else  s.push(i);
      }
    build(1,1,n);
    while(q--)
    {
        int ql,qr;
        scanf("%d %d",&ql,&qr);
        int _1=1e9,_2=-2;
        ask(1,1,n,ql,qr,_1,_2);
        if(_1==ql&&_2==qr)
          puts("Yes");
        else puts("No");
    }
    return 0;
}
```

