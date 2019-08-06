---
title: 'Codeforces #512 div2'
date: 2019-02-05 01:18:21
tags: codeforces
categories: codeforces
mathjax: true

---
#  A:

签到题。

---
<!-- more -->
# B:

判断点是否在矩形内,利用外积即可。

---
```
#include<bits/stdc++.h>
using namespace std;
int n,d,m;
int main()
{
  cin>>n>>d;
  cin>>m;
  for(int i=1;i<=m;i++)
  {
      int x,y;
      cin>>x>>y;
      int _1=(n-d)*(y-d)-(n-d)*x;
      int _2=(d-n)*(y-n+d)-(d-n)*(x-n);
      int _3=(-d)*y-d*(x-d);
      int _4=d*(y-n)-(-d)*(x-n+d);
      if(_1*_2>=0&&_3*_4>=0)
        cout<<"YES"<<endl;
      else cout<<"NO"<<endl;

  }
}
```
---
# C:

暴力。

---
```
#include<bits/stdc++.h>
using namespace std;
int n;
string a;
int main()
{
    cin>>n;
    cin>>a;
    int sum=0;
    string str="";
    for(int i=0; i<n; i++)
    {
        if(a[i]!='0') str+=a[i];
        sum+=a[i]-'0';
    }
    bool ok=true;
    bool flag=false;
    for(int i=1; i<=sum; i++)
    {
        int tem=0;
        int cnt=0;
        ok=true;

        for(int j=0; j<str.length(); j++)
        {
            tem+=str[j]-'0';
            if(tem>i)
            {
                ok=false;
                break;
            }
            if(tem==i)
            {
                tem=0;
                cnt++;
                if(j==str.length()-1) flag=true;
                continue;
            }
        }
        if(ok&&cnt>1&&flag)
        {
            break;
        }
        if(!flag) ok=false;
        if(cnt==1) ok=false;
    }
    if(ok) cout<<"YES"<<endl;
    else cout<<"NO"<<endl;

}
```
---
# D:

$由皮克定理可知，该三角形底乘高为整数 \\
则有2m/k \le b \le m \\
2n/k \le a \le n \\
观察2n/k，因为k \ge 2,令t=gcd(2n,k) \\
若t \ge 2,令a=2n/t,若t=1,令a=n。 $

---
```
#include<bits/stdc++.h>
using namespace std;
long long n,m,k;
int main()
{
    cin>>n>>m>>k;
    if(2*n*m%k!=0)
    {
        cout<<"NO"<<endl;
        return 0;
    }
    int t=__gcd(2*n,k);
    int a=2*n/t;
    int b=m*t/k;
    cout<<"YES"<<endl;
    if(t!=1)
    {
        cout<<0<<" "<<0<<endl;
        cout<<a<<" "<<0<<endl;
        cout<<0<<" "<<b<<endl;
    }
    else
    {
        cout<<0<<" "<<0<<endl;
        cout<<n<<" "<<0<<endl;
        cout<<0<<" "<<2*m/k<<endl;
    }
    return 0;
}
```
---
未完待续。。。
