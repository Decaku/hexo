---
title: 'Codeforces Round #514 (Div. 2)'
date: 2019-02-05 01:42:28
tags: codeforces
categories: codeforces
mathjax: true

---
# A:

$模拟时特判一下n为0即可$

---
<!-- more -->
```
#include<bits/stdc++.h>
using namespace std;
int n,l,a,ans;
int main()
{
    ans=0;
    scanf("%d %d %d",&n,&l,&a);
    if(n==0)
        ans=l/a;
    else
    {
        int s,t,_;
        scanf("%d %d",&s,&t);
        ans+=s/a;
        _=s+t;
        for(int i=2; i<=n; i++)
        {
            scanf("%d %d",&s,&t);
            ans+=(s-_)/a;
            _=s+t;
        }
        ans+=(l-_)/a;
    }
    printf("%d\n",ans);

}
```
---
# B:

对地图染色以后暴力。

---
```
#include<bits/stdc++.h>
using namespace std;
char mp[1005][1005];
bool vis[1005][1005];
int main()
{
    int n,m;
    cin>>n>>m;
    memset(vis,0,sizeof(vis));
    for(int i=1; i<=n; i++)
        for(int j=1; j<=m; j++)
            cin>>mp[i][j];

    for(int i=2; i<=n-1; i++)
        for(int j=2; j<=m-1; j++)
        {
            if(mp[i-1][j-1]=='.'||mp[i-1][j]=='.'||mp[i-1][j+1]=='.'||mp[i][j-1]=='.'||mp[i][j+1]=='.'
               ||mp[i+1][j-1]=='.'||mp[i+1][j]=='.'||mp[i+1][j+1]=='.')
                continue;
            else
            {
                vis[i-1][j-1]=1;
                vis[i-1][j]=1;
                vis[i-1][j+1]=1;
                vis[i][j-1]=1;
                vis[i][j+1]=1;
                vis[i+1][j-1]=1;
                vis[i+1][j]=1;
                vis[i+1][j+1]=1;
            }
        }
    bool ans=true;
    for(int i=1; i<=n; i++)
    {
        if(ans==false) break;
        for(int j=1; j<=m; j++)
        {
            if((mp[i][j]=='#'&&!vis[i][j])||(mp[i][j]=='.'&&vis[i][j]))
            {
                ans=false;
                break;
            }
        }
    }
    if(ans) cout<<"YES"<<endl;
    else cout<<"NO"<<endl;
    return 0;
}
```
---
# C:

$采取贪心策略尽早使gcd比1大,又相邻的奇数和偶数 \\
一定互素,所以先删奇数，剩下偶数不断除2变成奇数 \\
删掉，递归即可$

---
```
#include<bits/stdc++.h>
using namespace std;
const int maxn=1e6+5;
int ans[maxn],ans_;
int seq[maxn];
void solve(int n,int t)
{
    if(n==1)
    {
        ans[ans_++]=t;
        return;
    }
    if(n==2)
    {
        ans[ans_++]=t;
        ans[ans_++]=t*2;
        return;
    }
    if(n==3)
    {
        ans[ans_++]=t;
        ans[ans_++]=t;
        ans[ans_++]=3*t;
        return;
    }

    for(int i=1;i<=n;i++)
    {
        if(seq[i]&1)
            ans[ans_++]=t;
    }
    for(int i=1;i<=n/2;i++)
    {
        seq[i]=seq[i*2]/2;
    }
    solve(n/2,t*2);
}
int main()
{
    int n;
    cin>>n;
    for(int i=1;i<=n;i++)
        seq[i]=i;
    solve(n,1);
    for(int i=0;i<ans_-1;i++)
        printf("%d ",ans[i]);
    printf("%d\n",ans[ans_-1]);
}
```
---
未完待续。。。
