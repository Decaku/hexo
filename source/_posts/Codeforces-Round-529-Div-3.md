---
title: 'Codeforces Round #529 (Div. 3)'
date: 2019-02-05 05:48:28
tags: codeforces
categories: codeforces
mathjax: true

---

# A&B:

$A模拟,B两个case比较。$

---
<!-- more -->
```
#include<bits/stdc++.h>
using namespace std;
#define ll long long

int main()
{
    int n;
    string str;
    scanf("%d",&n);
    cin>>str;
    string ans="";
    int ct=1;
    for(int i=0; i<n; i+=ct)
    {
        ans+=str[i];
        ct++;
    }
    cout<<ans<<endl;
}
```
---
```
#include<bits/stdc++.h>
using namespace std;
#define ll long long
const int maxn=1e5+5;
int a[maxn];

int main()
{
    int n;
    scanf("%d",&n);
    for(int i=1; i<=n; i++)
        scanf("%d",&a[i]);
    sort(a+1,a+n+1);
    int ans=0;
    ans=min(a[n-1]-a[1],a[n]-a[2]);
    printf("%d\n",ans);
}
```
---

# C:

所有数都是可以拆的,贪心的不断把大数除2直到变成1，特判输入是1,1的情况。

---
```
#include<bits/stdc++.h>
using namespace std;
#define ll long long
int n,k;
vector<int>ans,v,t;
void ini()
{
    for(int i=1; i<=n; i*=2)
    {
        v.push_back(i);
    }
}

bool solve()
{
    while(1)
    {
        int tem=*(upper_bound(v.begin(),v.end(),n)-1);
        t.push_back(tem);
        n-=tem;
        if(!n) break;
    }
    if(t.size()>k) return false;
    else
    {
        int sz=t.size();
        while(sz<k)
        {
            t[0]/=2;
            if(t[0]==1)
            {
                ans.push_back(t[0]);
                ans.push_back(t[0]);
                t.erase(t.begin());
                sz++;
            }
            else
            {
                t.push_back(t[0]);
                t.push_back(t[0]);
                t.erase(t.begin());
                sz++;
            }
        }
    }
    return true;
}

int main()
{
    scanf("%d %d",&n,&k);
    ini();
    if(n&1)
    {
        ans.push_back(1);
        n--;
        k--;
    }
    if(n==0&&k==0)
    {
        puts("YES");
        puts("1");
    }
    else if(k>n)
    {
        puts("NO");
    }
    else
    {
        if(!solve())
            puts("NO");
        else
        {
            puts("YES");
            for(int i=0; i<ans.size(); i++)
                printf("%d ",ans[i]);
            for(int i=0; i<t.size(); i++)
                printf("%d ",t[i]);
        }
    }
}
```
---

# D:

$如果i的两个临接数是a和b且i的下一个数就是a,那么a的两个 \\
邻接数里一定有b，然后就可以建图了，输出答案一遍dfs即可。$

---
```
#include<bits/stdc++.h>
using namespace std;
#define ll long long
const int maxn=4e5+5;
vector<int>v[maxn];
int n;
struct Edge
{
    int nex,to;
} e[maxn];
int head[maxn],cnt,vis[maxn];
void ini()
{
    memset(head,-1,sizeof(head));
    memset(vis,0,sizeof(vis));
    cnt=-1;
}
void add_edge(int u,int v)
{
    e[++cnt].nex=head[u];
    e[cnt].to=v;
    head[u]=cnt;
}
void dfs(int u)
{
    printf("%d ",u);
    vis[u]=1;
    for(int i=head[u]; i!=-1; i=e[i].nex)
    {
        int v=e[i].to;
        if(vis[v]) continue;
        dfs(v);
    }
}
int main()
{
    scanf("%d",&n);
    for(int i=1; i<=n; i++)
    {
        int x,y;
        scanf("%d %d",&x,&y);
        v[i].push_back(x);
        v[i].push_back(y);
    }
    ini();
    for(int i=1; i<=n; i++)
    {
        int x=v[i][0],y=v[i][1];
        if(v[x][0]==y||v[x][1]==y)
        {
            add_edge(i,x);
            add_edge(x,y);
        }
        else
        {
            add_edge(i,y);
            add_edge(y,x);
        }
    }
    dfs(1);
    printf("\n");
}
```
---
# E:

正着反着各扫描一遍字符串处理4个数组，然后对于每个位置判断一下把它反转以后是否能使得字符串平衡。

---
```
include<bits/stdc++.h>
using namespace std;
#define ll long long
const int maxn=1e6+5;
char str[maxn];
int n;
int pre[maxn],prec[maxn],suf[maxn],sufc[maxn];
//pre[i]前i个字符的封闭度
//prec[i]前i个字符是否可以作前缀
//suf[i] i到n的字符封闭度
//sufc[i] i到n的字符是否可以做后缀
int main()
{
    scanf("%d",&n);
    scanf("%s",str+1);
    if(str[1]=='(')
    {
        pre[1]=1;
        prec[1]=1;
    }
    else
    {
        pre[1]=-1;
        prec[1]=0;
    }
    for(int i=2; i<=n; i++)
    {
        if(str[i]=='(') pre[i]=pre[i-1]+1;
        else  pre[i]=pre[i-1]-1;
        if(prec[i-1]&&pre[i]>=0) prec[i]=1;
        else  prec[i]=0;
    }
    if(str[n]==')')
    {
        suf[n]=1;
        sufc[n]=1;
    }
    else
    {
        suf[n]=-1;
        sufc[n]=0;
    }
    for(int i=n-1; i>=1; i--)
    {
        if(str[i]==')') suf[i]=suf[i+1]+1;
        else suf[i]=suf[i+1]-1;
        if(sufc[i+1]&&suf[i]>=0) sufc[i]=1;
        else sufc[i]=0;
    }
    int ans=0;
    prec[0]=sufc[n+1]=1;
    for(int i=1; i<=n; i++)
    {
        if(prec[i-1]&&sufc[i+1])
        {
            if(str[i]==')'&&suf[i+1]-pre[i-1]==1)
                ans++;
            if(str[i]=='('&&pre[i-1]-suf[i+1]==1)
                ans++;
        }
    }
    printf("%d\n",ans);
}
//( ( ( ( ) )
```
---
# F:

$直接求MST复杂度会爆炸,但是容易发现只有n条边可能在MST里。$

---
```
#include<bits/stdc++.h>
using namespace std;
#define ll long long
const ll maxn=2e5+5;
ll n,m;
ll f[maxn],W[maxn];
void ini()
{
    for(ll i=0;i<maxn;i++)
        f[i]=i;
}
ll Find(ll x)
{
    return f[x]==x?x:f[x]=Find(f[x]);
}
void join(ll a,ll b)
{
    f[Find(a)]=Find(b);
}
struct edge
{
    ll u,v,w;
} e[maxn<<2];
bool cmp(edge a,edge b)
{
    return a.w<b.w;
}
ll cnt;
int main()
{
    ini();
    scanf("%lld %lld",&n,&m);
    ll pos=1;
    for(ll i=1; i<=n; i++)
    {
        scanf("%lld",&W[i]);
        if(W[i]<W[pos]) pos=i;
    }
    for(ll i=1; i<=n; i++)
    {
        if(i!=pos)
        {
            e[++cnt].u=pos;
            e[cnt].v=i;
            e[cnt].w=W[pos]+W[i];
        }
    }
    for(ll i=1; i<=m; i++)
    {
        ll u,v,w;
        scanf("%lld %lld %lld",&u,&v,&w);
        e[++cnt].u=u;
        e[cnt].v=v;
        e[cnt].w=w;
    }
    sort(e+1,e+cnt+1,cmp);
    ll ans=0;
    for(ll i=1; i<=cnt; i++)
    {
        ll u=e[i].u,v=e[i].v,w=e[i].w;
        if(Find(u)!=Find(v))
        {
            join(u,v);
            ans+=w;
        }
    }
    printf("%lld\n",ans);
}
```

