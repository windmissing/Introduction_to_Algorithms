---
layout: post
title:  "HDOJ 1166 敌兵布阵"
category: ACM
tags: [acm, hdoj, 树状数组]
---

# 题目链接

http://acm.hdu.edu.cn/showproblem.php?pid=1166  

关键字：树状数组

# 题目分析

将一组数组s[N]

输入Query a b，输出SUM（sa + …… + sb）

输入Add a b，s[a] = s[a] + b

输入Sub a b，s[a] = s[a] - b

数组动态求和，明显的树状数组，调用树状数组模版：树状数组

# 代码

```c++
#include <iostream>
#include "TreeArray.h"
using namespace std;
int main()
{
    int t,cnt,i, n, b, c;
	char a[6];
	//调用 树状数组模版
	TreeArray Tra(1);
    scanf("%d",&t);
    for(cnt=1;cnt<=t;cnt++)
    {
		//树状数组初始化
		Tra.clear();
        printf("Case %d:\n",cnt);
		//输入数组的初始值
        scanf("%d",&n);
        for(i=1;i<=n;i++)
        {
            int x;
            scanf("%d",&x);
            Tra.modify(i,x);
        }
		//动态修改和求和
        while(scanf("%s",a))
        {
            if(strcmp(a,"End")==0)break;
            scanf("%d%d",&b,&c);
            if(strcmp(a,"Add")==0)Tra.modify(b, c);
            else if(strcmp(a,"Sub")==0)Tra.modify(b, -c);
            else printf("%d\n",Tra.sum(c)-Tra.sum(b-1));
        }
    }
    return 0;
}
```

总结：

原来用C++标准输入输出能过，现在TLE，要改用C的输入输出才行。

复习了一下树状数组

