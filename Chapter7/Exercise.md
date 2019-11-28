### 7.1 快速排序的描述

##### 7.1-1

A = {13 19 9 5 12 8 7 4 21 2 6 11}

==> A = {9 5 8 7 4 2 6 11 21 13 19 12}

==> A = {5 4 2 6 9 8 7 11 21 13 19 12}

==> A = {2 4 5 6 9 8 7 11 21 13 19 12}

==> A = {2 4 5 6 9 8 7 11 21 13 19 12}

==> A = {2 4 5 6 7 8 9 11 21 13 19 12}

==> A = {2 4 5 6 7 8 9 11 21 13 19 12}

==> A = {2 4 5 6 7 8 9 11 12 13 19 21}

==> A = {2 4 5 6 7 8 9 11 12 13 19 21}

==> A = {2 4 5 6 7 8 9 11 12 13 19 21}

##### 7.1-2

返回r

##### 7.1-2

修改PARTITION(A, p, r)，增加对A[i]==x时的处理。对于A[i]==x的数据，一半放在x左边，一半放在x右边

[算法过程](https://github.com/windmissing/exerciseForAlgorithmSecond/blob/master/src/chapter7/Exercise7_1_2.cpp)

[测试](https://github.com/windmissing/exerciseForAlgorithmSecond/blob/master/tst/chapter7/Exercise7_1_2Test.cpp)

##### 7.1-3

PARTITION()的具体过程如下：

(1)x<-A[r]，O(1)

(2)遍历数组，O(n)

(3)exchange，O(1)

因此运行时间为O(n)

#####7.1-4

修改PARTITION(A, p, r)，把L4改为do if A[j] >= x
  

### 7.2 快速排序的性能

##### 7.2-1

见《算法导论》7.4.1。

我的方法：

```
T(n)   = T(n-1) + O(n)
T(n-1) = T(n-2) + O(n-1)
  ……   = ……   + ……
T(2)   = T(1)   + O(2)
------------------------
T(n)   = T(1)   + O(n) + O(n-1) + …… + O(2)
= O(n^2)
```
##### 7.2-2

O(n^2)

##### 7.2-3

当数组A包含不同元素且按降序排序时，每次划分会划分成n-1个元素和1个元素这两个区域，即最坏情况。因此时间为O(n^2)

##### 7.2-4

基本有序的数列用快排效率较低

##### 7.2-5

若第一层的元素个数是n，那么会划分成n(1-a)个元素和na个元素这两个区域。0<a<=1/2 ==> na<=n(1-a)，因此只考虑n(1-a)。第t层元素个数为na^(t-1)。当na^(t-1)=1时划分结束。解得t=-lgn/lg(1-a)+1，大约是-lgn/lg(1-a)。

##### 7.2-6

可参考http://blog.163.com/kevinlee_2010/blog/static/16982082020112585946451/，
不过我没看懂
  
  

### 7.3 快速排序的随机化版本

##### 7.3-1

随机化不是为了提高最坏情况的性能，而是使最坏情况尽量少出现

##### 7.3-2

最坏情况下，n个元素每次都划分成n-1和1个，1个不用再划分，所以O(n)次

最好情况下，每次从中间划分，递推式N(n)=1+2*N(n/2)=O(n)
  

### 7.4 快速排序的分析

##### 7.4-1

没有找到关于这几个符号的定义

##### 7.4-2

见《算法导论》P88最佳情况划分

##### 7.4-3

令f(q) = q^2 + (n-q-1)^2
       = 2q^2 + 2(1-n)q + (n-1)^2

这是一个关于q的抛物线，且开口向上。因此q的取值离对称轴越远，f(q)的值就越大。

对称轴为q = -b/2a = (n-1)/2

当q=0或q=n-1时取得最大值

##### 7.4-4

见《算法导论》P7.4.2

##### 7.4-5

[算法过程](https://github.com/windmissing/exerciseForAlgorithmSecond/blob/master/src/chapter7/Exercise7_4_5.cpp)