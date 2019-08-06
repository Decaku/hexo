---
title: Codeforces Round 533 Div2
date: 2019-02-05 15:08:05
tags: codeforces
categories: codeforces
mathjax: true

---
# A:

暴力。

---
<!-- more -->
```
#include<bits/stdc++.h>
using namespace std;
#define ll long long
int a[1005];
int n;
int main()
{
    scanf("%d",&n);
    for(int i=1; i<=n; i++)
        scanf("%d",&a[i]);

    int t,ans=1e9;
    for(int i=1;i<=100;i++)
    {
        int p=0;
        for(int j=1;j<=n;j++)
        {
             if(a[j]<i-1)
                p+=i-1-a[j];
             else if(a[j]>i+1)
                p+=a[j]-(i+1);
        }
        if(p<ans)
        {
            ans=p;
            t=i;
        }
    }
    printf("%d %d\n",t,ans);

}
```
---
# B:

$统计26个字母的level，然后比较。$

---
```
#include<bits/stdc++.h>
using namespace std;
#define ll long long
const int maxn=2e5+5;
int cnt[30];
char str[maxn];
int n,k;
vector<char>v;

int main()
{
    memset(cnt,0,sizeof(cnt));
    v.clear();
    scanf("%d %d",&n,&k);
    scanf("%s",str);
    //int tem=0;
    for(int i=0; i<n; i++)
    {
        //tem++;
        //v.push_back(str[i]);
        if(v.empty()||v[0]==str[i])
        {
            v.push_back(str[i]);
            if(v.size()==k)
            {
                cnt[v[0]-'a']++;
                v.clear();
            }
        }
        else if(v[0]!=str[i])
        {
            v.clear();
            v.push_back(str[i]);
        }
    }
    int ans=-1;
    for(int i=0; i<=26; i++)
        ans=max(ans,cnt[i]);
    printf("%d\n",ans);
}
```
---
# D:

$计数dp，dp[i][j]表示前i个数的和模3等于j的方案数，答案是dp[n][0]。$

---
```
#include<bits/stdc++.h>
using namespace std;
#define ll long long
const int maxn=2e5+5;
const int mod=1e9+7;
ll a[maxn],n,l,r;
ll dp[maxn][3];
int main()
{
   scanf("%lld %lld %lld",&n,&l,&r);
   ll a=r/3-(l-1)/3;
   ll b=(r+2)/3-(l+2-1)/3;
   ll c=(r+1)/3-(l+1-1)/3;
   dp[1][0]=a;
   dp[1][1]=b;
   dp[1][2]=c;
   for(int i=2;i<=n;i++)
   {
      dp[i][0]=(dp[i-1][0]*a%mod+dp[i-1][1]*c%mod+dp[i-1][2]*b%mod)%mod;
      dp[i][1]=(dp[i-1][0]*b%mod+dp[i-1][1]*a%mod+dp[i-1][2]*c%mod)%mod;
      dp[i][2]=(dp[i-1][0]*c%mod+dp[i-1][1]*b%mod+dp[i-1][2]*a%mod)%mod;
   }
   printf("%lld",dp[n][0]);
}
```
---
# D:

$bfs,用vector[i]维护第i个人走了第j步以后增加的格子。$

---
```
#include<bits/stdc++.h>
using namespace std;
int n,m,p;
int s[11],a[1100][1100];
int d[4][2]= {1,0, -1,0, 0,1, 0,-1};
char str[1100];
vector<pair<int,int> >v[1100],t;
int ans[11];
bool bfs(int x)
{
    t.clear();
    for(int i=0; i<v[x].size(); i++)
        t.push_back(v[x][i]);
    v[x].clear();
    int sz=t.size();
    for(int i=0; i<sz; i++)
    {
        for(int j=0; j<4; j++)
        {
            int X=t[i].first,Y=t[i].second;
            if(X+d[j][0]<1||X+d[j][0]>n) continue;
            if(Y+d[j][1]<1||Y+d[j][1]>m) continue;
            if(a[X+d[j][0]][Y+d[j][1]]==0)
            {
                a[X+d[j][0]][Y+d[j][1]]=x;
                v[x].push_back(make_pair(X+d[j][0],Y+d[j][1]));
            }
        }
    }
    if(!v[x].size()) return 0;
    return 1;

}
int main()
{
    memset(a,0,sizeof(a));
    scanf("%d %d %d",&n,&m,&p);
    for(int i=1; i<=p; i++)
        scanf("%d",&s[i]);
    for(int i=1; i<=n; i++)
    {
        scanf("%s",str+1);
        for(int j=1; j<=m; j++)
        {
            if(str[j]=='#')
                a[i][j]=-1;
            else if(str[j]!='.')
            {
                a[i][j]=str[j]-'0';
                v[str[j]-'0'].push_back(make_pair(i,j));
            }
        }
    }


    while(1)
    {
        bool ok=false;
        for(int i=1; i<=p; i++)
            for(int j=1; j<=s[i]; j++)
            {
                if(!bfs(i)) break; //只要有一次为真 就会为真
                else ok=true;
            }
        if(!ok) break;
    }



    for(int i=1; i<=n; i++)
    {
        for(int j=1; j<=m; j++)
        {
            if(a[i][j]>0) ans[a[i][j]]++;
        }
    }
    for(int i=1; i<=p; i++)
        printf("%d ",ans[i]);
    printf("\n");
}
```
---
# E:

$对于两个type1之间的朋友，只会选择一个朋友去修改名字，那么 \\
可以把朋友作为结点，对于两个type1之间的结点，两两之间建立无 \\
向边，最后即求尽量多的点，之间没有边，问题转化为最大独立集。 \\
最大独立集等于补图最大团。$

---
```
#include<bits/stdc++.h>
using namespace std;
const int maxn=45;
int g[maxn][maxn];
int ans,cnt[maxn],group[maxn],n,m,vis[maxn];
bool dfs(int u,int pos)
{
    int i,j;
    for( i=u+1; i<=m; i++)
    {
        if(cnt[i]+pos<=ans)
            return 0;
        if(g[u][i])
        {
            for( j=0; j<pos; j++)
                if(!g[i][vis[j]])
                    break;
            if(j==pos)
            {
                vis[pos]=i;
                if(dfs(i,pos+1))
                    return 1;
            }
        }
    }
    if(pos>ans)
    {
        for(int i=0; i<pos; i++)
            group[i]=vis[i];
        ans=pos;
        return 1;
    }
    return 0;
}

void maxclique()
{
    ans=-1;
    for(int i=m; i>0; i--)
    {
        vis[0]=i;
        dfs(i,1);
        cnt[i]=ans;
    }
}

vector<string>v;
map<string,int> mp;
int tot;
int main()
{
    scanf("%d %d",&n,&m);
    for(int i=1; i<=n; i++)
    {
        int type;
        scanf("%d",&type);
        if(type==2)
        {
            string str;
            cin>>str;
            v.push_back(str);
            if(mp[str]==0) mp[str]=++tot;
            //mp[str]=++tot;
        }
        else
        {
            for(int i=0; i<v.size(); i++)
            {
                for(int j=0; j<i; j++)
                {
                    g[mp[v[i]]][mp[v[j]]]=1;
                    g[mp[v[j]]][mp[v[i]]]=1;
                }
            }
            v.clear();
            //mp.clear();
        }
    }
    if(v.size())
    {
        for(int i=0; i<v.size(); i++)
        {
            for(int j=0; j<i; j++)
            {
                g[mp[v[i]]][mp[v[j]]]=1;
                g[mp[v[j]]][mp[v[i]]]=1;
            }
        }
    }
   /*for(int i=1; i<=m; i++)
    {
        for(int j=1; j<=m; j++)
            cout<<g[i][j]<<" ";
        cout<<endl;
    }
   // return 0;
*/
    for(int i=1; i<=m; i++)
    {
        for(int j=1; j<=m; j++)
        {
            if(i!=j)
                g[i][j]^=1;

        }
    }
    maxclique();
    printf("%d\n",ans);
}
```

