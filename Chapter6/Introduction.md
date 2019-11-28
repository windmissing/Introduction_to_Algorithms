# 一、概念

#### 1.堆heap

堆的本质是一种数组对象，数组下标从1开始

堆可以被视作一棵完全二叉树，二叉树的层次遍历结果与数组元素的顺序对应，树根为A[1]。

对于数组中第i个元素，其对应在二叉树中的父母孩子结点位置的计算如下：

```
PARENT(i)
    return i/2
LEFT(i)
    return 2i
RIGHT(i)
    return 2i+1
```

#### 2.最大/小堆(max-heap/min-heap)

从二叉树的角度看，对于所有非root结点，满足`node->parent ≥ node`/`node->parent ≤ node`

从数组的角度看，对于所有下标大于1的元素，其下标为i，则满足`A[PARENT( i)] ≥ A[i]`/`A[PARENT( i)] ≤ A[i]`

#### 3.高度height

结点的高度：从结点到叶子所经过的边的数量，叶子结点的高度为0

二叉树的高度：树中高度最高的结点的高度，只有一个结点的树高度为0

堆的高度：把堆看所作二叉树时的高度


# 二、程序

#### 1.堆的结构

A[N]：堆数组

length[A]：数组中元素的个数

heap-size[A]：存放在A中的堆的元素个数

  
#### 2.在堆上的操作

（1）MAX-HEAPIFY(A, i)

（2）BUILD-MAX-HEAP(A)

（3）HEAPSORT(A)


#### 3.堆的应用

优先级队列

（1）HEAP-MAXIMUM(A)

（2）HEAP-INCREASE-KEY(A, i, key)

（3）HEAP-EXTRACT-KEY(A)

（4）MAX-HEAP-INSERT(A, key)

  
#### 4.堆代码

[产品代码](https://github.com/windmissing/exerciseForAlgorithmSecond/blob/master/src/chapter6/Heap.h)

[测试代码](https://github.com/windmissing/exerciseForAlgorithmSecond/blob/master/tst/chapter6/HeapTest.cpp)
  

#### 5.排序代码

[产品代码](https://github.com/windmissing/exerciseForAlgorithmSecond/blob/master/src/others/sort.cpp)

[测试代码](https://github.com/windmissing/exerciseForAlgorithmSecond/blob/master/tst/others/sortTest.cpp)

# 三、练习

### 6.1 堆

##### 6.1-1
最多2^(h+1) - 1， 最少2 ^ h（当树中只有一个结点时，高度是0）

##### 6.1-2

```
上一题结论 ==> 2^h <= n <= 2^(h+1) - 1

         ==> h <= lgn <= h + 1

         ==> lgn = h
```
##### 6.1-3

```
max-heap的定义
==>A[PARENT(i)]>=A[i]
==>A[1]>A[2],A[3]
==>A[1]>A[4],A[5],A[6],A[7]
==>……
==>the root of the subtree contains the largest value
```
##### 6.1-4

叶子上

##### 6.1-5

是最小堆或最大堆

##### 6.1-6

不是，7是6的左孩子，但7>6

##### 6.1-7

```
A[2i]、A[2i+1]是A[i]的孩子
==>若2i<=n&&2i+1<=n，则A[i]有孩子
==>若2i>n，则A[i]是叶子
==>the leaves are the nodes indexed by ?n/2 ? + 1, ?n/2 ? + 2, . . . , n
```

### 6.2 保持堆的性质

##### 6.2-1

A = {27,17,3,16,13,10,1,5,7,12,4,8,9,0}

A = {27,17,10,16,13,3,1,5,7,12,4,8,9,0}

A = {27,17,10,16,13,9,1,5,7,12,4,8,3,0}

##### 6.2-2

```
MIN-HEAPIFY(A, i)
 1    l <- LEFT(i)
 2    r <- RIGHT(i)
 3    if l <= heap-size[A] and A[l] < A[i]
 4        then smallest <- l
 5        else smallest <- i
 6    if r <= heap-size[A] and A[r] < [smallest]
 7        then smallest <- r
 8    if smallest != i
 9        then exchange A[i] <-> A[smallest]
 10                MIN_HEAPIFY(A, smallest)
```
 
##### 6.2-3

没有效果，程序终止

##### 6.2-4

i > heap-size[A]/2时，是叶子结点，也没有效果，程序终止

##### 6.2-5

我还是比较喜欢用C++，不喜欢用伪代码

```c++
void Heap::Max_Heapify(int i)
{
	int l = (LEFT(i)), r = (RIGHT(i)), largest;
	//选择i、i的左、i的右三个结点中值最大的结点
	if(l <= heap_size && A[l] > A[i])
		largest = l;
	else largest = i;
	if(r <= heap_size && A[r] > A[largest])
		largest = r;
	//如果根最大，已经满足堆的条件，函数停止
	//否则
	while(largest != i)
	{
		//根与值最大的结点交互
		swap(A[i], A[largest]);
		//交换可能破坏子树的堆，重新调整子树
		i = largest;
		l = (LEFT(i)), r = (RIGHT(i));
		//选择i、i的左、i的右三个结点中值最大的结点
		if(l <= heap_size && A[l] > A[i])
			largest = l;
		else largest = i;
		if(r <= heap_size && A[r] > A[largest])
			largest = r;
	}
}
```

##### 6.2-6

MAX-HEAPIFY中每循环一次，当前处理的结点的高度就会+1，最坏情况下，结点是根结点的时候停止，此时结点高度是logn，因此最坏运行时间是logn
  
  
### 6.3 建堆

##### 6.3-1

```
A = {5,3,17,10,84,19,6,22,9}
A = {5,3,17,22,84,19,6,10,9}
A = {5,3,19,22,84,17,6,10,9}
A = {5,84,19,22,3,17,6,10,9}
A = {84,5,19,22,3,17,6,10,9}
A = {84,22,19,5,3,17,6,10,9}
A = {84,22,19,10,3,17,6,5,9}
```
##### 6.3-2

因为MAX-HEAPIFY中使用条件是当前结点的左孩子和右孩子都是堆

假设对i执行MAX-HEAPIFY操作，当i=j时循环停止，结果是从i到j的这条路径上的点满足最大堆的性质，但是PARENT[i]不一定满足。

甚至有可能在满足A[PARENT(i)]>A[i]的情况下因为执行了MAX-HEAPIFY(i)而导致A[PARENT(i)]<A[i]，例如下图，

因此一定要先执行MAX-HEAPIFY(i)才能执行MAX-HEAPIFY(PARENT(i))

![](/image/e0677a6dad948f0ec318147a17122084.jpg)

[aaa](/image/e0677a6dad948f0ec318147a17122084.jpg)

##### 6.3-3

对于一个含有n个结点的堆，最多可以有f(h)个叶子结点

已知，当h=0时，f(h)=(n+1)/2

高度为h的结点是高度为h-1的结点的父结点，因此f(h) = (f(h-1) + 1) / 2

```
f(0) = (n + 1) / 2  = 1 + (n-1)/2
f(1) = (f(0) +1) / 2 = 1 + (n-1)/(2^2)
f(2) = (f(1) +1) / 2 = 1 + (n-1)/(2^3)
……
f(h) = 1 + (n-1) / (2 ^ (h+1))
```
因为f(h)必须是整数，所以

```
f(h) = floor[1 + (n-1) / (2 ^ (h+1)) ]
     = ceilling[(n-1) / (2 ^ (h+1))]
     ≤  ceilling[n / (2 ^ (h+1))]
```
得证：在任一含n个元素的堆中，至多有ceiling(n/(2^(h+1)))个高度为h的节点。

参考http://blog.csdn.net/lqh604/article/details/7381893

### 6.4 堆排序的算法

##### 6.4-1

A = {5,13,2,25,7,17,20,8,4}

A = {25,13,20,8,7,17,2,5,4}

A = {4,13,20,8,7,17,2,5,25}

A = {20,13,17,8,7,4,2,5,25}

A = {5,13,17,8,7,4,2,20,25}

A = {17,13,5,8,7,4,2,20,25}

A = {2,13,5,8,7,4,17,20,25}

A = {13,8,5,2,7,4,17,20,25}

A = {4,8,5,2,7,13,17,20,25}

A = {8,7,5,2,4,13,17,20,25}

A = {4,7,5,2,8,13,17,20,25}

A = {7,4,5,2,8,13,17,20,25}

A = {2,4,5,7,8,13,17,20,25}

A = {5,4,2,7,8,13,17,20,25}

A = {2,4,5,7,8,13,17,20,25}

A = {4,2,5,7,8,13,17,20,25}

A = {2,4,5,7,8,13,17,20,25}

A = {2,4,5,7,8,13,17,20,25}

##### 6.4-2

初始化：第一轮迭代之前，i=length[A]，则i=n。显然，循环不变式成立。

保持：每次迭代前，对于一个给定的i。假设循环不变式成立，则子数组A[1..i]是一个包含了A[1..n]中的i个最小元素的最大堆；而子数组A[i+1..n]包含了已排序的A[1..n]中的n-i个最大元素。经过3~5行代码的操作后，使得子数组A[1..i-1]是一个包含了A[1..n]中的i-1个最小元素的最大堆；子数组A[i..n]包含了已排序的A[1..n]中的n-i+1个最大元素。则可以保证i=i-1后的下一次迭代开始前，循环不变式成立。

终止：循环终止时，i=1。根据循环不变式，子数组A[i+1..n]即A[2..n]包含了已排序的A[1..n]中的n-1个最大元素，所以数组A是已排好序的。

##### 6.4-3

按递增排序的数组，运行时间是nlgn

按递减排序的数组，运行时间是n

#####6.4-4

堆排序算法中，对堆中每个结点的处理过程为：

（1）取下头结点，O(1)

（2）把最后一个结点移到根结点位置，O(1)

（3）对该结点执行MAX-HEAPIFY，最坏时间为O(lgn)

对每个结点处理的最坏时间是O(lgn)，每个结点最多处理一次。

因此最坏运行时间是O(nlgn)

### 6.5 优先级队列

##### 6.5-1

A = {15,13,9,5,12,8,7,4,0,6,2,1}

A = {1,13,9,5,12,8,7,4,0,6,2,1}

A = {13,1,9,5,12,8,7,4,0,6,2,1}

A = {13,12,9,5,1,8,7,4,0,6,2,1}

A = {13,12,9,5,6,8,7,4,0,1,2,1}

return 15

##### 6.5-2

A = {15,13,9,5,12,8,7,4,0,6,2,1}

A = {15,13,9,5,12,8,7,4,0,6,2,1,-2147483647}

A = {15,13,9,5,12,8,7,4,0,6,2,1,10}

A = {15,13,9,5,12,10,7,4,0,6,2,1,8}

A = {15,13,10,5,12,9,7,4,0,6,2,1,8}

##### 6.5-3

```
HEAP-MINIMUM(A)
1    return A[1]
HEAP-EXTRACR-MIN(A)
1    if heap-size[A] < 1
2        then error "heap underflow"
3    min <- A[1]
4    A[1] <- A[heap-size[A]]
5    heap-size[A] <- heap-size[A] - 1
6    MIN-HEAPIFY(A, 1)
7    return min
HEAP-DECREASE-KEY(A, i, key)
1    if key > A[i]
2        then error "new key is smaller than current key"
3    A[i] <- key
4    while i > 1 and A[PARENT(i)] > A[i]
5        do exchange A[i] <-> A[PARENT(i)]
6              i <- PARENT(i)
MIN-HEAP-INSERT
1    heap-size[A] <- heap-size[A] + 1
2    A[heap-size[A]] <- 0x7fffffff
3    HEAP-INCREASE-KEY(A, heap-size[A], key)
```
##### 6.5-4

要想插入成功，key必须大于这个初值。key可能是任意数，因此初值必须是无限小

#####6.5-6

FIFO：以进入队列的时间作为权值建立最小堆

栈：以进入栈的时间作为权值建立最大堆

#####6.5-7

```c++
void Heap::Heap_Delete(int i)
{
	if(i > heap_size)
	{
		cout<<"there's no node i"<<endl;
		exit(0);
	}
	int key = A[heap_size];
	heap_size--;
	if(key > A[i])   //最后一个结点不一定比中间的结点最
		Heap_Increase_Key(i, key);
	else
	{
		A[i] = key;
		Max_Heapify(i);
	}
}
```
  
##### 6.5-8

step1：取每个链表的第一个元素，构造成一个含有k个元素的堆

step2：把根结点的值记入排序结果中。

step3：判断根结点所在的链表，若该链表为空，则go to step4，否则go to step5

step4：删除根结点，调整堆，go to step2

step5：把根结点替换为原根结点所在链表中的第一个元素，调整堆，go to step 2

[产品代码](https://github.com/windmissing/exerciseForAlgorithmSecond/blob/master/src/chapter6/Exercise6_5_8.cpp)

[测试代码](https://github.com/windmissing/exerciseForAlgorithmSecond/blob/master/tst/chapter6/Exercise6_5_8Test.cp)
  
# 四、思考题

### 6-1 用插入方法建堆

```c++
void Heap::Build_Max_Heap()
{
	heap_size = 1;
	//从堆中最后一个元素开始，依次调整每个结点，使符合堆的性质
	for(int i = 2; i <= length; i++)
	    Max_Heap_Insert(A[i]);
}
```
答：

a）A = {1,2,3};

b）MAX-HEAP-INSERT的过程如下：

加入大小为-0x7FFFFFFF的新结点，O(1)

将该值调整为key,最坏情况下为O(lgn)

对每个结点都要执行一次插入操作，因此最坏时间为O(nlgn)

### 6-2 对d叉堆的分析

a）根结点是A[1]，根结点的孩子是A[2],A[3],……，A[d+1]

PARENT(i) = (i - 2 ) / d + 1  

CHILD(i, j ) = d * (i - 1) + j + 1  

b）lgn/lgd  

c）HEAP-EXTRACR-MAX(A)与二叉堆的实现相同，其调用的MAX-HEAPIFY(A, i)要做部分更改，时间复杂度是O(lgn/lgd * d)  

```
MAX-HEAPIFY(A, i)
1    largest <- i
2    for j <- 1 to d
3        k <- CHILD(i, j)  
4        if k <= heap-size[A] and A[k] > A[largest]  
5            largest <- k
6    if largest != i  
7    then exchange A[i] <-> A[largest]  
8             MAX-HEAPIFY(A, largest)  
```
d）和二叉堆的实现完全一样，时间复杂度是O(lgn/lgd)  

e）和二叉堆的实现完全一样，时间复杂度是O(lgn/lgd) 
  
  
### 6-3 Young氏矩阵

见[算法导论 6-3 Young氏矩阵](http://blog.csdn.net/mishifangxiangdefeng/article/details/7976452)
