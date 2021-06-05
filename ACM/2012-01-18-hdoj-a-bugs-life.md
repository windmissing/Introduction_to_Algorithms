---
layout: post
title:  "HDU1829 A Bug's Life 并查集"
category: [ACM]
tags: []
---

并查集模板：http://blog.csdn.net/mishifangxiangdefeng/article/details/7165351


```c++
/* 
HDU1829A Bug's Life 并查集的应用 
这题不是判断是否在同一集合，而是判断是否在不同的集合 
*/  
#include <iostream>  
#include "UFS.h"  
using namespace std;  
  
int oppo[MAXN];//记录系第一个BUG的异性所在的集合的编号  
int main()  
{  
    int t;  
    int n,m,i,j;  
    UFS ufs;  
    scanf("%d",&t);  
    for(j=1;j<=t;j++)  
    {  
        bool f=0;  
        memset(oppo, 0, sizeof(oppo));  
        ufs.clear();  
        scanf("%d%d",&n,&m);  
  
        for(i=0;i<m;i++)  
        {  
            int a,b;  
            scanf("%d%d",&a,&b);  
            int x = ufs.Find(a), y = ufs.Find(b);  
            if(x==y)f=1;//如果在同一集合，肯定是Suspicious bugs   
            else        //如果在不同集合，就把对方加入自己的异性集合  
            {  
                int min,p,q;  
                //如果自己的异性集合为空  
                if(oppo[x]==0)  
                    oppo[x]=y;  
                //如果自己的异性集合不为空  
                else  
                    ufs.Union(y, oppo[x]);  
                if(oppo[y]==0)  
                    oppo[y]=x;  
                else  
                    ufs.Union(x, oppo[y]);  
            }  
        }  
        printf("Scenario #%d:\n",j);  
        if(f)printf("Suspicious bugs found!\n");  
        else printf("No suspicious bugs found!\n");  
        if(i!=t)printf("\n");  
    }  
    return 0;  
}  
```
