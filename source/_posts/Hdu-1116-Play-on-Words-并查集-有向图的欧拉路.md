---
title: Hdu 1116 Play on Words 并查集+有向图的欧拉路
date: 2019-02-04 15:09:39
tags: [并查集, 欧拉路]
categories: 并查集
---
# 描述:

类似于成语接龙，给若干个单词，问能否组成一个排列，使得一个单词的尾字母和后面一个单词的首字母相同。例如acm，malform，mouse这三个单词就满足题意。

---
<!-- more -->
# 思路:

把每个单词看成一条边，取首字母和尾字母，作为起点和终点。建图，图上有26个点，每输入一个单词，就在对应的两点间建立一条有向边。要判断输入的单词是否满足题意，其实就是判断图中是否存在一条欧拉路径。如果这个图不只包含一个连通分量，那么一定不满足题意。连通分量用并查集维护。

下面的问题就是判断有向图的欧拉路径。如果不是回路，那么图中只有一个点的出度比入度大1（起点），一个点的出度比入度小1（终点），其它点出度等于入度。如果是欧拉回路，则所有的点出度等于入度。并且如果存在出度入度的差值大于1的点也一定不能形成欧拉路。

---
```
#include<bits/stdc++.h>
using namespace std;
int p[30];

int Find(int x)
{
    if(x!=p[x])
        p[x]=Find(p[x]);
    return p[x];
}

void join(int a,int b)
{
    if(Find(a)!=Find(b))
        p[Find(a)]=Find(b);
}


int main()
{
    //freopen("input.txt","r",stdin);
    //freopen("output.txt","w",stdout);
    int t,num,num1,num2;
    cin>>t;
    int indeg[30],outdeg[30];
    bool vis[30],flag;
    while(t--)
    {
        num=num1=num2=0;
        flag=true;
        int n;
        cin>>n;
        char str[1000];
        for(int i=0; i<26; i++)
        {
            p[i]=i;
            vis[i]=false;
            outdeg[i]=indeg[i]=0;
        }
        
        
        for(int i=0; i<n; i++)
        {
            cin>>str;
            join(str[0]-'a',str[strlen(str)-1]-'a');
            indeg[str[0]-'a']++;
            outdeg[str[strlen(str)-1]-'a']++;
            vis[str[0]-'a']=true;
            vis[str[strlen(str)-1]-'a']=true;
        }
        
        for(int i=0; i<26; i++)
        {
            if(p[i]==i&&vis[i])
                num++;
            if(indeg[i]-outdeg[i]==1)
                num1++;
            if(outdeg[i]-indeg[i]==1)
                num2++;
            if(indeg[i]-outdeg[i]>1||outdeg[i]-indeg[i]>1)
                flag=false;
        }
        
        if(flag==true&&num==1&&(num1==num2==0||num1==num2==1))
        {
            cout<<"Ordering is possible."<<endl;
        }
        else cout<<"The door cannot be opened."<<endl;
    }
}
```

