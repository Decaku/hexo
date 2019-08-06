---
title: 2018ICPC徐州网络赛 Maze Designer 最大生成树+LCA
date: 2019-02-04 20:47:22
tags: [最大生成树, LCA]
categories: LCA
mathjax: true
---
# 描述:

要求删除一些边以后使剩下一棵生成树,并且使得删除的边权和最小,然后询问树上2点的最短距离。

---
<!-- more -->
# 思路:

$ 先求原图的最大生成树，然后求LCA。$

---
```
#include<bits/stdc++.h>
using namespace std;
typedef long long LL;
const int N=250005;
struct node{
    int v,dis,next;
}edges[N<<1];
int head[N],e;
int id[N]; //节点第一次被遍历的顺序
int dis[N]; //节点到根节点的距离
int RMQ[N*3][20];
int curID;
//F[i]表示第i个遍历的节点
//B[i]表示F[i]在树中的深度
int F[N*3],B[N*3];
int root;
void init()
{
    e = 0; curID = 0;
    memset(head,-1,sizeof(head));
}
void Add (int u,int v,int dis)
{
    edges[e].v=v;
    edges[e].dis=dis;
    edges[e].next=head[u];
    head[u]=e++;
}

void DFS (int u,int p,int Dep,int d)
{
    int i,v;
    curID++;
    F[curID]=u;
    B[curID]=Dep;
    id[u]=curID;
    dis[u]=d;
    for (i=head[u];i!=-1;i=edges[i].next){
        v=edges[i].v;
        if (v==p) continue;
        DFS(v,u,Dep+1,d+1);
        curID++;
        F[curID]=u;
        B[curID]=Dep;
    }
}

void initRMQ ()
{
    int i,j,x,y;
    for (i=1;i<=curID;i++)
        RMQ[i][0]=i;
    for (j=1;(1<<j)<=curID;j++)
        for (i=1;i+(1<<j)-1<=curID;i++){
            x=RMQ[i][j-1];
            y=RMQ[i+(1<<(j-1))][j-1];
            RMQ[i][j]=B[x]<B[y]?x:y;
        }
}

int getLCA (int a,int b)
{
    int k,x,y;
    a=id[a];b=id[b];
    if (a>b)
        k=a,a=b,b=k;
    k=log(1.0+b-a)/log(2.0);
    x=RMQ[a][k];
    y=RMQ[b-(1<<k)+1][k];
    return B[x]<B[y]?F[x]:F[y];
}

int dist(int x,int y)
{
    return dis[x] +dis[y] - 2*dis[getLCA(x,y)];
}

struct Edg{
    int u,v;
    LL dist;
    bool operator<(const Edg &rhs)const{return dist>rhs.dist;}
};

struct Kruskal{
    int n,m;
    Edg edges[N<<1];
    int fa[N];
    int findset(int x){ return fa[x]==-1?x:fa[x]=findset(fa[x]); }

    void init(int n){
        this->n=n;
        m=0;
        memset(fa,-1,sizeof(fa));
    }

    void AddEdge(int u,int v,LL dist){
        edges[m++] = (Edg){u,v,dist};
    }

    void kruskal(){
        LL sum=0;
        int cnt=0;
        sort(edges,edges+m);
        for(int i=0;i<m;i++){
            int u=edges[i].u, v=edges[i].v;
            if(findset(u) != findset(v)){
                Add(u,v,1);
                Add(v,u,1);
                sum +=edges[i].dist;
                fa[findset(u)] = findset(v);
                if(++cnt>=n-1) return ;
            }
        }
        return ;
    }
}G;

int main()
{
    int n,m;
    char c1,c2;
    LL w1,w2;
    while(scanf("%d %d",&n,&m)==2){
        init();
        G.init(n*m+1);
        for(int i=1;i<=n;++i){
            for(int j=1;j<=m;++j){
                scanf("\n%c %lld %c %lld",&c1,&w1,&c2,&w2);
                if(c1=='D'){
                    G.AddEdge((i-1)*m+j,i*m+j,w1);
                }
                if(c2 == 'R'){
                    G.AddEdge((i-1)*m+j,(i-1)*m+j+1,w2);
                }
            }
        }
        G.kruskal();
        root=1;
        DFS(root,0,0,0);
        initRMQ();
        int Q; scanf("%d",&Q);
        int x1,y1,x2,y2;
        while(Q--){
            scanf("%d %d %d %d",&x1,&y1,&x2,&y2);
            int u = (x1-1)*m+y1, v = (x2-1)*m+y2;
            printf("%d\n",dist(u,v));
        }
    }
    return 0;
}
```

