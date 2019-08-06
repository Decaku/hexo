---
layout: post
title: 'Codeforces Round #519 Div2'
date: 2019-02-05 03:58:52
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
int sum,n,tem,mx;

int main()
{
    scanf("%d",&n);
    mx=-1;
    for(int i=0;i<n;i++)
    {
        scanf("%d",&tem);
        mx=max(tem,mx);
        sum+=tem;
    }
    int ans=max((2*sum)/n+1,mx);
    printf("%d\n",ans);
}
```
---
# B:

差分数组后就是求字符串可能的循环节长度，暴力。

---
```
#include<bits/stdc++.h>
using namespace std;
int n,a[1005],b[1005],ans[1005],_ans;
int main()
{
    scanf("%d",&n);
    a[0]=0;
    for(int i=1;i<=n;i++)
        scanf("%d",&a[i]);
    for(int i=1;i<=n;i++)
        b[i]=a[i]-a[i-1];
    for(int i=1;i<=n;i++)
    {
        bool ok=true;
        for(int j=i+1;j<=n;j+=i)
        {
            for(int k=j;k<=j+i-1&&k<=n;k++)
            {
                if(b[k]!=b[k-i])
                {
                    ok=false;
                    break;
                }
            }
            if(!ok) break;
        }
        if(ok) ans[++_ans]=i;
    }
    printf("%d\n",_ans);
    for(int i=1;i<=_ans;i++)
        printf("%d ",ans[i]);
    printf("\n");
}
```
---
# C:

$操作完了以后一定可以让所有a在b的前面,所以假设 \\
前i长的字符串已经是最小字典序了,那么如果第i+1个 \\
字符是a,前i个字符就要倒过来,再反转前i+1个字符. \\
$

---
```
#include<bits/stdc++.h>
using namespace std;
int ans[1005];
int main()
{
    string str;
    cin>>str;
    for(int i=1;i<str.size();i++)
    {
        if(str[i]=='a') ans[i-1]^=1, ans[i]^=1;
    }
    for(int i=0;i<str.size()-1;i++)
        printf("%d ",ans[i]);
    printf("%d\n",ans[str.size()-1]);
}
```
---
# D:

$预处理一个a数组,a_i=j代表在所有字符串中i的后一个字符 \\
都是j,然后根据a数组建一棵树,在树上统计每一条链上的结点 \\
个数就行。$

---
```
#include<bits/stdc++.h>
using namespace std;
const int maxn=1e5+5;

int a[maxn],head[maxn],vis[maxn];
int n,m,tem,pre,cnt;

struct Edge
{
    int to,nex;
} edge[2*maxn];

void add_edge(int u,int v)
{
    edge[++cnt].to=v;
    edge[cnt].nex=head[u];
    head[u]=cnt;
    edge[++cnt].to=u;
    edge[cnt].nex=head[v];
    head[v]=cnt;
}

int dfs(int u)
{
    vis[u]=1;
    int ret=0;
    for(int i=head[u]; i!=-1; i=edge[i].nex)
    {
        int v=edge[i].to;
        if(vis[v])
            continue;
        ret+=dfs(v)+1;
    }
    return ret;
}

int main()
{
    memset(head,-1,sizeof(head));
    cnt=-1;
    scanf("%d %d",&n,&m);
    for(int i=1; i<=m; i++)
    {
        scanf("%d",&pre);
        for(int j=2; j<=n; j++)
        {
            scanf("%d",&tem);
            if(a[pre]==0)
                a[pre]=tem;
            else if(a[pre]!=tem)
                a[pre]=-1;
            pre=tem;
        }
        a[pre]=-1;
    }
    for(int i=1; i<=n; i++)
    {
        if(a[i]!=-1)
            add_edge(i,a[i]);
    }
    long long ans=0;
    for(int i=1; i<=n; i++)
    {
        if(!vis[i])
        {
            int tem =dfs(i)+1+1;
            ans+=1ll*tem*(tem-1)/2;
        }
    }
    printf("%lld\n",ans);
}
```
---
未完待续。。。
