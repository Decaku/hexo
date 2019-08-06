---
title: Poj1679 The Unique MST
date: 2019-02-04 23:32:58
tags: 次小生成树
categories: 最小生成树

---
# 描述: 

判断最小生成树是否唯一。

---
<!-- more -->
# 思路:

求次小生成树和最小生成树比较。

对边按照边权排序，边权相同按照边的序号升序排列，然后求最小生成树的边的集合。
之后在边权相同时按照边的序号降序排列，又可以得到一组生成树中边的序号集合。
如果集合不同说明生成树不唯一。

---
```
#include<stdio.h>
#include<string.h>
#include<algorithm>
#include<iostream>
#define M 107
#define inf 0x3f3f3f
using namespace std;
int g[M][M],path[M][M];
int dist[M],pre[M],vis[M];
bool used[M][M];
int n,m,mst;

void init()
{
    for(int i=0; i<=n; i++)
        for(int j=i+1; j<=n; j++)
            g[i][j]=g[j][i]=inf;

}

int prime()
{
    int mst=0;
    memset(path,0,sizeof(path));
    memset(vis,0,sizeof(vis));
    memset(used,0,sizeof(used));
    vis[1]=1;
    for(int i=1; i<=n; i++)
    {
        dist[i]=g[1][i];
        pre[i]=1;
    }
    for(int i=1; i<n; i++)
    {
        int u=-1;
        for(int j=1; j<=n; j++)
        {
            if(!vis[j])
                if(u==-1||dist[j]<dist[u])
                    u=j;
        }
        used[u][pre[u]]=used[pre[u]][u]=true;
        mst+=g[pre[u]][u];
        vis[u]=1;
        for(int j=1; j<=n; j++)
        {
            if(vis[j]&&j!=u)
                path[j][u]=path[u][j]=max(path[j][pre[u]],dist[u]);
            if(!vis[j])
                if(dist[j]>g[u][j])
                {
                    dist[j]=g[u][j];
                    pre[j]=u;
                }
        }
    }
    return mst;
}

int second_tree()
{
    int res=inf;
    for(int i=1; i<=n; i++)
        for(int j=1; j<=n; j++)
            if(i!=j&&!used[i][j])
                res=min(res,mst-path[i][j]+g[i][j]);
    return res;
}

int main()
{
    int t,a,b,c;
    scanf("%d",&t);
    while(t--)
    {
        scanf("%d%d",&n,&m);
        init();
        for(int i=1; i<=m; i++)
        {
            scanf("%d%d%d",&a,&b,&c);
            g[a][b]=g[b][a]=c;
        }
        mst=prime();
        int second_mst=second_tree();
        if(mst!=second_mst)
            cout<<mst<<endl;
        else cout<<"Not Unique!"<<endl;
    }
}
```
---
```
#include<iostream>
#include<string>
#include<algorithm>
using namespace std;

int ans;
const int N=10000+1;
struct edge{int u,v,w,id;}g[N];
int f[N];//记得初始化
int find(int a) {
	return a == f[a]?a:f[a] = find(f[a]);
} 
//对边集排序后，对所有边调用join函数就可以得到最小生成树
bool join(const edge &e){
	if(find(e.u) !=find(e.v)){
		f[find(e.u)] = find(e.v);
		ans+=e.w;
		return true;
//ans =max(ans,e.w)，cnt++;
	}
	return false;
}

bool cmp1(edge a,edge b){
	if(a.w!=b.w)return a.w<b.w;
	else return a.id<b.id;
}
bool cmp2(edge a,edge b){
	if(a.w!=b.w)return a.w<b.w;
	else return a.id>b.id;
}

int main(){
	int t;
	while(cin>>t)while(t--){
		int n,m,u,v,w;
		cin>>n>>m;
		for(int i=0;i<m;i++){
			cin>>g[i].u>>g[i].v>>g[i].w;
			g[i].id=i;
		}
		
		string state1,state2;

		ans=0;
		sort(g,g+n,cmp1);
		for(int i=0;i<=n;i++)f[i]=i;
		for(int i=0;i<m;i++)if(join(g[i]))state1.push_back(g[i].id);

		ans=0;
		sort(g,g+n,cmp2);
		for(int i=0;i<=n;i++)f[i]=i;
		for(int i=0;i<m;i++)if(join(g[i]))state2.push_back(g[i].id);
		
		if(state1!=state2)cout<<"Not Unique!"<<endl;
		else cout<<ans<<endl;
	}
}
```


