---
layout: post
title: " 算法导论 15-3 编辑距离"
category: 算法导论
tags: [算法, 算法导论, 动态规划]
---

#### 题目概述

有六种操作，分别是复制（copy）、替换（replace）、删除（delete）、插入（insert）、交换（twiddle）、消灭（kill）。  
将这六种操作任意组合（可以重复或者没有）得到一个操作序列。  
操作序列的输入是一个字符串，操作的输出是另一个字符串。  
例如字符串algorithm，经过以下操作序列，得到字符串altruistic。  
![](http://img.my.csdn.net/uploads/201208/30/1346306961_5723.gif)  
一个字符串src，变成另一个字符串dst，可以很多种方法。  
题目要求对于给定的src和dst，找到一种最优的操作序列，包含最少的操作个数，使src变成dst。输出这个操作序列。操作序列中操作的个数就是src到dst的编辑距离。  

<!-- more -->

下面具体解释这六种操作的内容即i和j的含义：  
i是src的下标，j是dst的下标。操作开始前，i = j = 0。  
令x为任意的字符  
copy: dst[j] = src[i], i++, j++  
replace: dst[j] = x, i++, j++  
delete: i++  
insert: dst[j] = x, j++  
twiddle: dst[j] = src[i+1], dst[j+1] = src[i], i += 2, j += 2  
kill: 消灭src中剩下的字符。如果执行这个操作，则它必是最的操作。  

#### 思考

对于DP问题，首先要想清楚的是初始化过递推。题目中i和j的递增给了很好的提示。  
定义c[i,j]为src的子串src[0,i]变化为dst的子串dst[0,j]的过程的编辑距离。  
那么c[src.length(), dst.length()]即src到dst的编辑距离。  
这里只是希望操作的个数最少，在操作个数相同的情况下，用什么操作是没有差别的，即每个操作的代价是一样的，即cost(operation) = 1

##### （1）初始化

```
c[0,0]=0
c[i,0] = i * cost(delete)
c[0,j] = j * cost(insert)
```

##### （2）递推

令c[i, j] = a，根据每个operation的规则，有以下推理  

```
copy: dst[j] = src[i], i++, j++ ==> c[i+1, j+1] = a + cost(copy)
replace: dst[j] = x, i++, j++  ==> c[i+1, j+1] = a + cost(replace)
delete: i++ ==>  c[i+1, j] = a + cost(delete) 
insert: dst[j] = x, j++  ==> c[i, j+1] = a + cost(insert)
twiddle: dst[j] = src[i+1], dst[j+1] = src[i], i += 2, j += 2  ==> c[i+2, j+2] = a + cost(twiddle)
kill: 消灭src中剩下的字符。如果执行这个操作，则它必是最的操作。
```

##### （3）反推

以上推理是基于现在的结果预测后面的结果。  
然后现在的结果并不能决定后面的结果，但现在的结果一定来自于前面的结果。  
因此要转换思考方向，将递推进行反推，这一步是DP的难点。  
![](http://img.my.csdn.net/uploads/201208/30/1346314160_1922.gif)  

##### （4）最后的操作kill

c[i][j] = MIN(c[m,n], MIN(c[i,n]+cost(kill)))， 其中0<=i<m

#### 代码

[源代码](https://github.com/windmissing/exerciseForAlgorithmSecond/blob/master/src/chapter15/Exercise15_3.cpp)  

#### DNA对齐问题

DNA对齐问题可以看作是带权值的编辑距离问题。  
在编辑距离问题中，所有的操作都看作是一样，即cost(operation) = 1  
现在把其中一操作加上权值，求最小值改成求最大值，就是DNA对齐问题了。  

```
copy : 1
replace : -1
insert : -2
delete ： 去掉
twiddle : 去掉
kill ： 去掉
```

#### 原题

![](http://img.my.csdn.net/uploads/201208/30/1346306943_9355.gif)
![](http://img.my.csdn.net/uploads/201208/30/1346306961_5723.gif)  
![](http://img.my.csdn.net/uploads/201208/30/1346306980_6765.gif)  
![](http://img.my.csdn.net/uploads/201208/30/1346306998_5928.gif) 



##### 思考题

[算法导论-15-1-双调欧几里得旅行商问题](http://blog.csdn.net/mishifangxiangdefeng/article/details/7918983)  
[算法导论-15-2-整齐打印](http://windmissing.github.io/%E7%AE%97%E6%B3%95%E5%AF%BC%E8%AE%BA/2012-08/algorithm-15-2-tidy-print.html)  
[算法导论-15-3-编辑距离](http://windmissing.github.io/%E7%AE%97%E6%B3%95%E5%AF%BC%E8%AE%BA/2012-08/edit-distance.html)  
[算法导论-15-4-计划一个公司聚会](http://blog.csdn.net/mishifangxiangdefeng/article/details/7930830)  
[ 算法导论-15-6-在棋盘上移动](http://blog.csdn.net/mishifangxiangdefeng/article/details/7931438)  
[算法导论-15-7-达到最高效益的调度](http://blog.csdn.net/mishifangxiangdefeng/article/details/7932349)  