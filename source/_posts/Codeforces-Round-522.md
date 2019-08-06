---
title: 'Codeforces Round #522 '
date: 2019-02-05 05:17:01
tags: codeforces
categories: codeforces
---
# A:

贪心。

---
<!-- more -->
```
#include<bits/stdc++.h>
using namespace std;
int cnt[101];
int main()
{
    memset(cnt,0,sizeof(cnt));
    int n,k;
    scanf("%d %d",&n,&k);
    for(int i=1; i<=n; i++)
    {
        int tem;
        scanf("%d",&tem);
        cnt[tem]++;
    }
    int _cnt=-1e9;
    for(int i=1; i<=100; i++)
    {
        _cnt=max(_cnt,cnt[i]);
    }
    int tem=ceil(1.0*_cnt/k);
    int ans=0;
    for(int i=1; i<=100; i++)
    {
        if(!cnt[i]) continue;
        ans+=tem*k-cnt[i];
    }
    printf("%d\n",ans);
}
```
---
# B:

贪心。

---
```
#include<bits/stdc++.h>
using namespace std;
const int inf=1e9;
string str;
int main()
{
    cin>>str;
    int len=str.length();
    int _len=len;
    bool ok=false;
    int _1,_2=inf;
    while(1)
    {
        for(int i=1; i<=len; i++)
        {
            if(len/i>20||len%i!=0||i>5) //i是组数
                continue;
            else if(i<_2)
            {
                _1=len;
                _2=i;
            }
        }
        if(len==100)
            break;
        len++;
    }

    int a=_1-_len;
    int b=a;
    printf("%d %d\n",_2,_1/_2);//_1/_2是每行的长度,_2是行数
    for(int i=1; i<=_2; i++)
    {
        for(int j=1; j<=_1/_2; j++)
        {
            if(j==_1/_2&&a>0)
            {
                cout<<"*";
                a--;
                break;
            }
            int pos=(i-1)*(_1/_2)+j-1-(b-a);
            cout<<str[pos];
        }
        cout<<endl;
    }
}
```
---

# C:

$dp[i][j]代表第i个位置能否放j,直接转移 \\
因为要打印路径，再记录一下每个状态的前驱。$

---
```
#include<bits/stdc++.h>
using namespace std;
const int maxn=1e5+5;
int n;
int a[maxn],dp[maxn][6],pre[maxn][6];//pre[i][j]保存dp[i][j]的前一个状态

void dfs(int i,int j)//i个位置选j
{
    if(i==1)
    {
        cout<<j<<" ";
        return ;
    }
    dfs(i-1,pre[i][j]);
    cout<<j<<" ";
}
int main()
{
    memset(dp,0,sizeof(dp));
    scanf("%d",&n);
    for(int i=1; i<=n; i++)
        scanf("%d",&a[i]);
    for(int j=1;j<=5;j++) dp[1][j]=1;
    for(int i=2; i<=n; i++)
    {
        if(a[i]==a[i-1])
        {
            for(int j=1; j<=5; j++)
            {
                for(int k=1; k<=5; k++)//k是前一个
                    if(dp[i-1][k]&&j!=k)
                        dp[i][j]=1, pre[i][j]=k;
                //pre[i][j]=
            }
        }
        else if(a[i]>a[i-1])
        {
            for(int j=1; j<=5; j++)
            {
                for(int k=1; k<=5; k++)
                    if(dp[i-1][k]&&j>k)
                        dp[i][j]=1, pre[i][j]=k;
            }
        }
        else if(a[i]<a[i-1])
        {
            for(int j=1; j<=5; j++)
            {
                for(int k=1; k<=5; k++)
                    if(dp[i-1][k]&&j<k)
                        dp[i][j]=1, pre[i][j]=k;
            }
        }
    }
    int ans=0,tem;
    for(int j=1; j<=5; j++)
        if(dp[n][j])
            ans=1,tem=j;
    if(!ans)
        cout<<-1<<endl;
    else
    {
        dfs(n,tem);
        cout<<endl;
    }
}
```
---

# D:

$猜了一下，应该只有五种case,都算出来取最小值即可。$

---
```
#include<bits/stdc++.h>
using namespace std;
double a,b,c;
double x_1,y_1,x_2,y_2;
double dis(double a,double b,double c,double d)
{
    return sqrt((a-c)*(a-c)+(b-d)*(b-d));
}
int main()
{
    scanf("%lf %lf %lf",&a,&b,&c);
    scanf("%lf %lf %lf %lf",&x_1,&y_1,&x_2,&y_2);
    double ans=abs(x_1-x_2)+abs(y_1-y_2);
    double p=(-1*c-b*y_1)/a,q=(-1*c-a*x_1)/b,r=(-1*c-b*y_2)/a,s=(-1*c-a*x_2)/b;
    double tem=abs(p-x_1)+abs(r-x_2)+dis(p,y_1,r,y_2);
    ans=min(ans,tem);

    tem=abs(p-x_1)+abs(s-y_2)+dis(p,y_1,x_2,s);
    ans=min(ans,tem);

    tem=abs(q-y_1)+abs(r-x_2)+dis(x_1,q,r,y_2);
    ans=min(ans,tem);

    tem=abs(q-y_1)+abs(s-y_2)+dis(x_1,q,x_2,s);
    ans=min(ans,tem);
    printf("%.10lf\n",ans);
}
```
---

# E:

$如果只有2种物品，ans最大可能是n，超过两种，能够鉴别出 \\
的砝码必须质量都相等。所以可以先做一次背包，求出i个砝码 \\
组成质量为j的方案数，有一个优化是dp过程中方案超过2就没意 \\
义了，它不可能是答案。然后对每种物品枚举数量，检查方案数 \\
是否只有一种，更新答案。要注意的是ans是n的情况要加特判。$

---
```
#include<bits/stdc++.h>
using namespace std;
int n,a[110],dp[10005][110];//dp[i][j]表示用j个物品正好装满容量为i的背包的方案数。
map<int,int>m;
vector<pair<int,int> >v;

int main()
{
    scanf("%d",&n);
    for(int i=1; i<=n; i++)
    {
        scanf("%d",&a[i]);
        m[a[i]]++;
    }
    for(map<int,int>::iterator it=m.begin(); it!=m.end(); it++)
    {
        v.push_back(*it);
    }
    dp[0][0]=1;

    for(vector<pair<int,int> >::iterator it=v.begin(); it!=v.end(); it++)
    {
        int tem=it->first;
        int cnt=it->second;
        for(int v=10000; v>0; v--)
        {
            for(int i=1; i<=n; i++)
            {
                for(int t=1; t<=cnt&&t<=i&&v-tem*t>=0; t++)
                    dp[v][i]+=dp[v-tem*t][i-t],dp[v][i]=min(dp[v][i],2);
            }
        }
    }
    int ans=1;
    for(vector<pair<int,int> >::iterator it=v.begin(); it!=v.end(); it++)
    {
        int tem=it->first;
        int cnt=it->second;
        for(int t=1; t<=cnt; t++)
        {
            if(dp[tem*t][t]==1)
            {
                if(v.size()==2&&t==cnt)
                {
                    printf("%d\n",n);
                    return 0;
                }
                else ans=max(ans,t);
            }
        }
    }
    printf("%d\n",ans);
}
```
