---
title: 2018Hdu暑期多校3 L Visual Cube
date: 2019-02-04 23:43:19
tags: 模拟
categorie: 模拟

---
# 描述:

打印一个长方体。

---
<!-- more -->
# 思路:

有意思的模拟。

---
```
#include<bits/stdc++.h>
#include<iostream>
using namespace std;
typedef long long ll;
char m[85][85];

int main()
{
    int t,a,b,c,tem;
    scanf("%d",&t);
    while(t--)
    {
        scanf("%d%d%d",&a,&b,&c);
        int A=2*a+1+2*b;
        int B=2*b+2*c+1;
        for(int i=1; i<=2*b; i++)
            for(int j=1; j<=2*b-i+1; j++)
                m[i][j]='.';
        int k=A;
        for(int i=2*c+2; i<=B; i++)
        {
            for(int j=k; j<=A; j++)
            {
                m[i][j]='.';
            }
            k--;
        }

        for(int i=2*b+1; i<=B; i+=2)
            for(int j=1; j<=2*a+1; j++)
            {
                if(j%2==1) m[i][j]='+';
                else  m[i][j]='-';
            }
        for(int i=2*b+2; i<=B-1; i+=2)
            for(int j=1; j<=2*a+1; j++)
            {
                if(j%2==1) m[i][j]='|';
                else  m[i][j]='.';
            }
        for(int i=1; i<=2*b; i++)
            for(int j=2*b-i+2; j<=2*b-i+2+2*a+1; j++)
            {

                if(i%2==1&&j%2==1) m[i][j]='+';
                else if(i%2==1&&j%2==0) m[i][j]='-';
                else if(i%2==0&&j%2==1) m[i][j]='.';
                else m[i][j]='/';
            }
        tem=B;
        for(int i=2*a+1; i<=A; i++)
        {
            for(int j=2*b+1+tem-B; j<=tem; j++)
            {
                if(i%2==1&&j%2==1) m[j][i]='+';
                else if(i%2==1&&j%2==0) m[j][i]='|';
                else if(i%2==0&&j%2==0)m[j][i]='/';
                else m[j][i]='.';

            }
            tem--;
        }
        for(int i=1; i<=B; i++)
        {
            for(int j=1; j<=A; j++)
            {
                cout<<m[i][j];
            }
            cout<<endl;
        }
    }
}
```
