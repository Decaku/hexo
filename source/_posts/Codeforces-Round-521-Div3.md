---
title: 'Codeforces Round #521 Div3'
date: 2019-02-05 05:31:47
tags: codeforces
categories: codeforces
mathjax: true

---
# A:

模拟。

---
<!-- more -->
```
#include<bits/stdc++.h>
using namespace std;
#define ll long long
int main()
{
    int t;
    scanf("%d",&t);
    ll a,b,k,ans;
    while(t--)
    {
        scanf("%lld %lld %lld",&a,&b,&k);
        ans=0;
        ans+=(k+1)/2*a;
        ans-=(k-(k+1)/2)*b;
        cout<<ans<<endl;
    }
}
```
---
# B:

贪心。

---
```
#include<iostream>
#include<cstdio>
#include<vector>
typedef long long ll;
using namespace std;
int n,a[200],vis[200];
vector<int>v;
int main()
{
    scanf("%d",&n);
    for(int i=1; i<=n; i++)
        scanf("%d",&a[i]);
    for(int i=2; i<=n-1; i++)
    {
        if(!a[i]&&a[i-1]&&a[i+1])
            v.push_back(i);
    }

    int k;
    if(v.empty())
        k=0;
    else
    {
        k=1;
        vis[v[0]]=1;
        for(int i=1; i<v.size(); i++)
        {
            if(v[i]-v[i-1]==2&&vis[v[i-1]]) continue;
            k++;
            vis[v[i]]=1;
        }
    }
    printf("%d\n",k);
}
```
---
# C:

作为和的数一定最大。

---
```
#include<iostream>
#include<cstdio>
#include<vector>
#include<algorithm>
typedef long long ll;
using namespace std;
const int maxn=2e5+5;
int n;
struct Node
{
    int id,w;
}a[maxn];
bool cmp(Node a,Node b)
{
    return a.w<b.w;
}
ll sum;
vector<int>v;
int main()
{
    scanf("%d",&n);
    for(int i=1;i<=n;i++)
        scanf("%d",&a[i].w),sum+=a[i].w,a[i].id=i;
    sort(a,a+n+1,cmp);
    ll ans=0;
    for(int i=1;i<n;i++)
    {
       ll tem=sum;
       if(tem-a[i].w-a[n].w==a[n].w)
          v.push_back(a[i].id),ans++;
    }
    if(sum-a[n].w-a[n-1].w==a[n-1].w)
        v.push_back(a[n].id),ans++;
    printf("%d\n",ans);
    for(int i=0;i<v.size();i++)
        printf("%d ",v[i]);
    printf("\n");
}
```
---

# D:

二分删掉的数在原数组里出现的次数。

---
```
#include<bits/stdc++.h>
using namespace std;
const int maxn=2e5+5;
int a[maxn],cnt[maxn],n,k,_cnt;
vector<int>v;
bool check(int mid)
{
    int res=0;
    for(int i=1; i<=n;)
    {
        if(res>=k) return true;
        res+=cnt[a[i]]/mid;
        i+=cnt[a[i]];
    }
    if(res>=k) return true;
    return false;
}

bool cmp(int x,int y)
{
    if(cnt[x]==cnt[y]) return x<y;
    return cnt[x]>cnt[y];
}


int main()
{
    scanf("%d %d",&n,&k);
    _cnt=n/k;
    for(int i=1; i<=n; i++) scanf("%d",&a[i]),cnt[a[i]]++;
    sort(a+1,a+n+1,cmp);
    int l=1,r=_cnt;
    while((r-l)>1)
    {
        int mid=(l+r)/2;
        if(check(mid))
        {
            l=mid;
        }
        else r=mid-1;
    }
     if(check(r)) l=r;
    int res=0;

    for(int i=1; i<=n;)
    {
        if(res>=k) break;
        for(int j=1; j<=cnt[a[i]]/l; j++)
        {
            v.push_back(a[i]);
        }
        res+=cnt[a[i]]/l;
        i+=cnt[a[i]];
    }

    for(int i=0; i<k; i++)
    {
        printf("%d ",v[i]);
    }
    printf("\n");
}
```
---

# E:

$记录每个数的次数后sort,对每个位置的查找都二分。$

---
```
#include<bits/stdc++.h>
using namespace std;
const int maxn=2e5+5;
int a[maxn],n;
map<int,int>mp;
vector<int>v;

int main()
{
    scanf("%d",&n);
    for(int i=1; i<=n; i++)
    {
        scanf("%d",&a[i]);
        mp[a[i]]++;
    }
    for(map<int,int>::iterator it=mp.begin(); it!=mp.end(); it++)
    {
        v.push_back(it->second);
    }
    sort(v.begin(),v.end());
    int ans=-1;
    for(int i=1; i<=n; i++)
    {
        int x=i;
        int d=0,tem=0;
        while(1)
        {
            int pos=lower_bound(v.begin()+d,v.end(),x)-v.begin();
            if(pos==v.size()) break;
            tem+=x;
            x<<=1;
            d=pos+1;
        }
        ans=max(ans,tem);
    }
    printf("%d\n",ans);
}
```
---
# F&G:

$dp过程中要用到滑窗最值,hard version用单调队列优化就好。$

---
```
#include<bits/stdc++.h>
using namespace std;
#define ll long long
const ll inf=1e15;
const ll maxn=5005;
ll n,k,x;
ll a[maxn],dp[maxn][maxn];//dp[i][j]前j个数选i个且必选第j个数能取得的最大值
ll q[maxn][maxn],l,r; //q[i-1][l]代表dp[i-1][j]到dp[i-1][j-k+1]的最大值

//dp[i][j]=max(dp[i-1][t])+a[j] (j-k+1<=t<j)
int main()
{
    scanf("%lld %lld %lld",&n,&k,&x);
    for(ll i=1; i<=n; i++)
        scanf("%lld",&a[i]);
    for(ll i=1; i<maxn; i++)
        for(ll j=1; j<maxn; j++)
            dp[i][j]=-inf;
    for(ll j=1; j<=k; j++)
        dp[1][j]=a[j];

    for(ll i=2; i<=x; i++)
    {
        l=0,r=-1;
        for(ll j=1; j<=n; j++)
        {
            if(l<=r) dp[i][j]=dp[i-1][q[i-1][l]]+a[j];

            while(l<=r&&dp[i-1][j]>=dp[i-1][q[i-1][r]])
                r--;
            q[i-1][++r]=j;
            if(l<=r&&q[i-1][l]<j-k+1)
                l++;
        }
    }
    ll ans=-1;
    for(ll j=n; j>n-k&&j>=1; j--)
        ans=max(ans,dp[x][j]);
    if(ans)
        printf("%lld\n",ans);
    else
        puts("-1");
}
```