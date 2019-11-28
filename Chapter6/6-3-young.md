#### 一、题目

![](http://windmissing.github.io/images_for_gitbook/Introduction_to_Algorithms/1.gif)  

#### 二、思考

最小Young氏矩阵和最小堆的思想差不多，可以通过比较两者的同异来理解Young氏矩阵

##### 不同点：

header 1            | min-Heap                  | min-Young
---                 |---                        | ---
堆顶（最小值）      | H[1]                      | Y[i][j]
最后一个元素的位置  | H[N]                      | Y[N][N]
最后一个元素        | 不一定是最大值            | 一定是最大值
parent              | H[i]的parent是H[i/2]      | Y[i][j]的parent是Y[i-1][j]和Y[i][j-1]
child	            | H[i]的child是H[i*2]和H[i*2+1] | Y[i][j]的child是Y[i+1][j]和Y[i][j+1]
find(x)	            | 从H[1]开始遍历 | 从西南角开始，或当前值小于key，则向东走，否则向北走
 	 	 

##### 相同点：

1.堆顶是最小值所在的位置

2.parent的值<=child的值

3.空条件：堆顶元素值为INIT

4.满条件：最后一个元素的值不为INIT

5.delete：插入到最后一个元素的位置，然后向parent调整

6.extract：提取堆顶元素，将堆顶元素置为INIT，然后child调整

#### 三、练习

##### a）

可以有多种画法

```
2   3   4   5  
8   9   12    
14  16    
```

##### c）  

提取Y[1][1]，并用ox7FFFFFFF填充。然后向右下调整  

```
YOUNG-EXTRACR-MIN(Y)  
1    if  Y[1][1] == 0X7FFFFFFF  
2        then error "heap underflow"  
3    min <- Y[1][1]  
4    A[1][1] <- 0X7FFFFFFF  
5    MAX-HEAPIFY(Y, 1, 1)  
6    return min  
//递归  
MIN-YOUNGIFY(Y, i, j)  
 1    min <- Y[i][j]  
 2    mini <- i  
 3    minj <- j  
 4    if i+1 < m and Y[i+1][j] < min  
 5        mini <- i+1  
 6        minj <- j  
 7        min <- Y[i+1][j]  
 8    if j+1 < n and Y[i][j+1] < min  
 9        mini <- i  
10        minj <- j+1  
11        min <- Y[i][j+1]  
12    if i != mini or j != minj  
13        exchange Y[i][j] <-> Y[mini][minj]  
14        MIN-YOUNGIFY(Y, mini, minj)  
d）  
//若表未满，插入到右下角，然后向左上调整  
MIN-YOUNG-INSERT(Y, key)  
 1    if Y[m][n] < 0x7fffffff  
 2        then error "young overflow"  
 3    Y[m][n] <- key  
 4    flag <-  true  
 5    i <- m  
 6    j <- n  
 7    max <- key  
 8    maxi <- i  
 9    maxj <- j  
10    while true  
11        if i > 1 and max < Y[i-1][j]  
12            maxi <- i - 1  
13            maxj <- j  
14            max <- Y[i-1][j]  
15        if j > 1 and max < Y[i][j-1]  
16            maxi <- i  
17            maxj <- j-1  
18            max <- Y[i][j-1]  
19        if max == Y[i][j]  
20            break  
21        exchange Y[maxi][maxj] <-> Y[i][j]  
22        i <- maxi  
23        j <- maxj  
24        max <- Y[i][j]  
```

##### d)

将新元素加入到矩阵的Y[m][n]处，然后逐步向上调整

参考产品代码`errorType Young::insert(int key)`

##### e）

排序过程为：  
    
step1:取下矩阵Y[1][1]元素，O(1)  

step2:将Y[1][1]置为0x7FFFFFFF，O(1)  

step3:调整矩阵，O(n)  

对所有n^2^个结点各执行以上step1~3一次，因此时间为O(n^3^)  

##### f）  
    
从左下角开找，若比它小，则向上，则比它大，则向右找  

```
FIND-YOUNG-KEY(Y, key)  
 1    i <- m  
 2   j <- 0  
 3   while i >= 1 and j <= n  
 4       if Y[i][j] = key  
 5       then return true  
 6       else if Y[i][j] < key  
 7       then j++  
 8       else if Y[i][j] > key  
 9       then i--  
10   return false  
```

#### 四、代码

[头文件](https://github.com/windmissing/exerciseForAlgorithmSecond/blob/master/include/chapter6/Young.h)

[产品代码](https://github.com/windmissing/exerciseForAlgorithmSecond/blob/master/src/chapter6/Young.cpp)
