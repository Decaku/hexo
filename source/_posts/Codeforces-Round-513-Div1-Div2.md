---
title: 'Codeforces Round #513 Div1+Div2'
date: 2019-02-05 01:32:50
tags: codeforces
categories: codeforces
mathjax: true

---
# A:

签到题。

---
<!-- more -->
```
#include<bits/stdc++.h>
using namespace std;
string str;
int main()
{
   int n;
   cin>>n;
   cin>>str;
   int cnt=0;
   for(int i=0;i<n;i++)
   {
       if(str[i]=='8')
        cnt++;
   }
   cnt=min(cnt,n/11);
   cout<<cnt<<endl;
}
```
---
# B:

$ 考虑从n的最低位开始更新答案，如果最低位小于等于8, \\
就从下一位拿1过来，ans+10。$

---
```
#include<bits/stdc++.h>
#define ll long long
using namespace std;

ll solve(ll n){
    ll ans=0;
    while(n){
        int d=n%10;
        n/=10;
        if(d<=8&&n){
            d=10;
            n--;
        }
        ans+=d;
    }
    return ans;
}

int main(){

    ll n;
    while(cin>>n){
        cout<<solve(n)<<endl;
    }
}
```
---
# C:

$ C矩阵的每一个子矩阵的和都是数组A和B的一个连续子段和相乘所得。 \\
所以可以考虑预处理A和B的所有连续子段和，然后把B的子段和按从小 \\
到大排序，每次枚举A的一个子段和a,可以在B的子段和里二分找x/a。 \\
然后就是询问子段和在0-x/a里最长的一段是多少，这个询问可以预处 \\
理，询问的时候是O(1)。所以总的复杂度是O(n^2log(n))。$

---
```
#include<bits/stdc++.h>
using namespace std;

const int maxn=2005;
int a[maxn],b[maxn];
int n,m,cnta,cntb;
int suma[maxn],sumb[maxn];


struct node{
    int len,w;
    bool operator <(const node&other)const{
        return w<other.w;
    };
}nodea[maxn*(maxn+1)/2],nodeb[maxn*(maxn+1)/2];
int lenb[maxn*(maxn+1)/2];


inline void ini(){
    for(int i=1;i<=n;i++)suma[i]=suma[i-1]+a[i];
    for(int i=1;i<=m;i++)sumb[i]=sumb[i-1]+b[i];
    cnta=cntb=0;
    for(int i=1;i<=n;i++){
        for(int j=i;j<=n;j++){
            nodea[cnta].len=j-i+1;
            nodea[cnta].w=suma[j]-suma[i-1];
            cnta++;
        }
    }
    for(int i=1;i<=m;i++){
        for(int j=i;j<=m;j++){
            nodeb[cntb].len=j-i+1;
            nodeb[cntb].w=sumb[j]-sumb[i-1];
            cntb++;
        }
    }
    
    sort(nodeb,nodeb+cntb);
    for(int i=0;i<cntb;i++)lenb[i]=max(lenb[i-1],nodeb[i].len);
    
}

int main(){
    cin>>n>>m;
    for(int i=1;i<=n;i++)scanf("%d",a+i);
    for(int i=1;i<=m;i++)scanf("%d",b+i);
    int x;cin>>x;
    
    ini();
    int ans=0;
    for(int i=0;i<cnta;i++){
        int t=upper_bound(nodeb,nodeb+cntb,node{0,x/nodea[i].w})-1-nodeb;
        ans=max(ans,lenb[t]*nodea[i].len);
    }
    cout<<ans<<endl;
}
```
---
未完待续。。。
