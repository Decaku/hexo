---
title: Edu Codeforces Round 53
date: 2019-02-05 04:18:42
tags: codeforces
categories: codeforces
mathjax: true

---
# A:

扫描长度为2的字符串就好。

---
<!-- more -->
```
#include<bits/stdc++.h>
using namespace std;
int a[30];
int main()
{
   bool flag=true;
   int n;
   string str;
   cin>>n;
   cin>>str;
   for(int i=0;i<n-1;i++)
   {
       if(str[i]!=str[i+1])
       {
           cout<<"YES"<<endl;
           cout<<str[i]<<str[i+1]<<endl;
           flag=false;
           break;
       }
   }
   if(flag) cout<<"NO"<<endl;
}
```
---
# B:

用栈模拟。

---
```
#include<bits/stdc++.h>
using namespace std;
int a[30];
int main()
{
   bool flag=true;
   int n;
   string str;
   cin>>n;
   cin>>str;
   for(int i=0;i<n-1;i++)
   {
       if(str[i]!=str[i+1])
       {
           cout<<"YES"<<endl;
           cout<<str[i]<<str[i+1]<<endl;
           flag=false;
           break;
       }
   }
   if(flag) cout<<"NO"<<endl;
}
```
---
# C:

$先记录两个数组a,b,a_i表示从起点开始i条指令能到达的位置,b_i \\
记录从终点开始对i条指令反向后能够到达的位置。二分修改 \\
区间的长度,所以区间左端点扫描,对区间右端点二分,check的时 \\
候只要根据区间长度是否比区间首尾的曼哈顿距离大且差值是偶 \\
数来二分即可,复杂度是nlog(n)。$

---
```
#include<bits/stdc++.h>
using namespace std;
#define maxn 200005
char str[maxn];
struct Point
{
    int x,y;
} s[maxn],e[maxn];
int main()
{
    int n,x,y;
    scanf("%d",&n);
    scanf("%s",str);
    scanf("%d %d",&x,&y);
    if((abs(x)+abs(y))>n||(n-abs(x)-abs(y))%2!=0)
        printf("%d\n",-1);
    else
    {
        int s_x=0,s_y=0;
        s[0].x=0,s[0].y=0;
        for(int i=0; i<n; i++)
        {
            if(str[i]=='U')
                s_y++, s[i+1].x=s_x, s[i+1].y=s_y;
            if(str[i]=='D')
                s_y--, s[i+1].x=s_x, s[i+1].y=s_y;
            if(str[i]=='L')
                s_x--, s[i+1].x=s_x, s[i+1].y=s_y;
            if(str[i]=='R')
                s_x++, s[i+1].x=s_x, s[i+1].y=s_y;
        }
        if(s[n].x==x&&s[n].y==y)
            printf("0\n");
        else
        {
            s_x=x,s_y=y;
            e[0].x=x,e[0].y=y;
            int cnt=0;
            for(int i=n-1; i>=0; i--)
            {
                if(str[i]=='U')
                    s_y--, e[++cnt].x=s_x, e[cnt].y=s_y;
                if(str[i]=='D')
                    s_y++, e[++cnt].x=s_x, e[cnt].y=s_y;
                if(str[i]=='L')
                    s_x++, e[++cnt].x=s_x, e[cnt].y=s_y;
                if(str[i]=='R')
                    s_x--, e[++cnt].x=s_x, e[cnt].y=s_y;
            }
            int ans=1e9;
            for(int i=1; i<=n; i++)
            {
                int l=i,r=n,mid;
                while(l<r)
                {
                    int mid=(l+r)/2;
                    int p_x=s[i-1].x,p_y=s[i-1].y;
                    int q_x=e[n-mid].x,q_y=e[n-mid].y;
                    int dis=abs(p_x-q_x)+abs(p_y-q_y);
                    if(mid-i+1>=dis&&(mid-i+1-dis)%2==0)
                        r=mid;
                    else
                        l=mid+1;
                }
                int p_x=s[i-1].x,p_y=s[i-1].y;
                int q_x=e[n-l].x,q_y=e[n-l].y;
                int dis=abs(p_x-q_x)+abs(p_y-q_y);
                if(l-i+1>=dis&&(l-i+1-dis)%2==0)
                    ans=min(ans,l-i+1);
            }
            printf("%d\n",ans);
        }
    }
}
```
---
# D:

经过一次循环以后可以取模,所以可以暴力莽过去。

---
```
#include<bits/stdc++.h>
using namespace std;
#define ll long long
const int maxn=2e5+5;
ll n,t,ans,sum,_;
int a[maxn];
int main()
{
    scanf("%lld %lld",&n,&t);
    for(int i=1; i<=n; i++)
        scanf("%d",&a[i]);
    while(1)
    {
        ll tem=t,cnt=0,sum=0;
        for(int i=1;i<=n;i++)
        {
            if(tem>=a[i])
            {
                cnt++;
                tem-=a[i];
                sum+=a[i];
            }
        }
        if(cnt==0) break;
        ans+=t/sum*cnt;
        t%=sum;
    }

    printf("%lld\n",ans);
}
```
---
未完待续。。。