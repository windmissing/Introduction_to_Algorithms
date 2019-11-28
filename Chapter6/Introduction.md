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

