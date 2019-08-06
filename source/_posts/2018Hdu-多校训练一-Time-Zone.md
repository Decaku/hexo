---
title: 2018Hdu 多校训练一 Time Zone
date: 2019-02-04 16:06:42
tags: 模拟
categories: 模拟
---
# 描述:

模拟时区，时区可以是小数。

---
<!-- more -->
# 思路:

大力模拟。

---
```
#include<cstdio>
#include<cstring>
#include<iostream>
#include<algorithm>
#include<cmath>

using namespace std;
int p,a,b,zs,xs;////直接从字符串里提取小数会有误差，所以这里分两部分提取出来
char str[15];
int main()
{
    scanf("%d",&p);
    while(p--)
    {
        scanf("%d%d%s",&a,&b,&str);
        if(str[5]=='.')zs=str[4]-'0',xs=str[6]-'0';

        else if(str[6]=='.')  zs=(str[4]-'0')*10+str[5]-'0',xs=str[7]-'0';
        else if(str[5]=='\0')  zs=str[4]-'0',xs=0;
        else if(str[6]=='\0')  zs=(str[4]-'0')*10+str[5]-'0',xs=0;
        if(str[3]=='-')  zs=-zs,xs=-xs;

        if(zs>=8)
        {
            if(xs*6+b>=60)
            {
                b=(xs*6+b)%60;
                a=(a+1+zs-8+24)%24;
            }
            else b=xs*6+b,a=(a+zs-8+24)%24;
            if(a>=10&&b>=10)  printf("%d:%d\n",a,b);
            else if(a<10&&b<10)  printf("0%d:0%d\n",a,b);
            else if(a<10&&b>=10)  printf("0%d:%d\n",a,b);
            else if(a>=10&&b<10)  printf("%d:0%d\n",a,b);
        }
        else if(zs<8)
        {
            int h=(80-(zs*10+xs))*6/60;
            int m=(80-(zs*10+xs))*6-h*60;
            if(m>b)
            {
                b=b+60-m;
                a=(a-1-h+24)%24;
            }
            else b=b-m,a=(a-h+24)%24;
            if(a>=10&&b>=10)  printf("%d:%d\n",a,b);
            else if(a<10&&b<10)  printf("0%d:0%d\n",a,b);
            else if(a<10&&b>=10)  printf("0%d:%d\n",a,b);
            else if(a>=10&&b<10)  printf("%d:0%d\n",a,b);
        }
    }
}
```
