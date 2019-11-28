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
  

