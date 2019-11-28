### 7-1 Hoare划分的正确性

a）  

A = {13 19 9 5 12 8 7 4 11 2 6 21}    

==> A = {6 19 9 5 12 8 7 4 11 2 13 21}    

==> A = {6 2 9 5 12 8 7 4 11 19 13 21}    

==> A = {4 2 9 5 12 8 7 6 11 19 13 21}    

==> A = {4 2 5 9 12 8 7 6 11 19 13 21}    

==> A = {2 4 5 9 12 8 7 6 11 19 13 21}    

==> A = {2 4 5 6 12 8 7 9 11 19 13 21}    

==> A = {2 4 5 6 7 8 12 9 11 19 13 21}    

==> A = {2 4 5 6 7 8 9 12 11 19 13 21}    

==> A = {2 4 5 6 7 8 9 12 11 13 19 21} 
 
b)自己写的，很乱，凑合看吧

主要证明以下几点：

（1）do repeat j<-j-1 until A[j]<=x

这个repeat中，第一次执行L6时p<=j<=r，最后一次执行L6时p<=j<=r

证明：

1.第一次执行L6时p<=j<=r。为了区分，j'=j-1，L6中的j用j'表示。

第一次进入while循环时，j=r+1，j'=r，满足p<=j<=r。

若不是第一次进入while循环，j<=r且j>p。因为如果j=p，在上一次while循环中L9的if不能通过，已经return了。因此p<=j<r-1，满足p<=j<=r。

2.最后一次执行L6时p<=j<=r，即要证明在A[p..r]中存在j'满足j'<=j且A[j]<=x

若第一次进入while循环，j'=p满足条件

若不是第一次进入while循环，在上一次while循环中交换过去的那个元素满足条件

（2）do repeat i<i+1 until A[i]>=x

这个repeat中，第一次执行L8时p<=i<=r，最后一次执行L8时p<=i<=r

证明：证明方法与（1）类似

c)根据b可知返回值p<=j<=r，这里只需证明j!=r

若A[r]>x,L5和L6的循环不会在j=r时停止，因此返回值j!=r

若A[r]<=x，只有在第一次进入while循环时，L5和L6的循环在j=r时停止。因为是第一次进入while循环，A[i]=A[p]=x，L7和L8的循环会在i=p时停止。显然会第二次进入while循环，此时j<r，因此返回值j!=r

d)题目写错了，应该是A[p..j]中的每个元素都小于或等于A[j+1..r]中的每个元素

结束时，A[p..i-1]中的元素都小于x，A[j+1..r]中的元素都大于x，命题得证

e)

```c++
int Hoare_Partition(int *A, int p, int r)    
{    
    int x = A[p], i = p - 1, j = r + 1;    
    while(true)    
    {    
        do{j--;}    
        while(A[j] > x);    
        do{i++;}    
        while(A[i] < x);    
        if(i < j)    
            swap(A[i], A[j]);    
        else return j;    
        Print(A, 12);    
    }    
}    
void Hoare_QuickSort(int  *A, int p, int r)    
{    
    if(p < r)    
    {    
        int q = Hoare_Partition(A, p, r);    
        Hoare_QuickSort(A, p, q-1);    
        Hoare_QuickSort(A, q+1, r);    
    }    
} 
```

### 7-2 对快速排序算法的另一种分析 

a)

```
               1 + 2 + …… + n       n + 1
    E[Xi] = -------------------- = -------
	                n                 2
```
                    
b)后面几题表示完全看不懂


### 7-3 Stooge排序

```c++
void Stooge_Sort(int *A, int i, int j)  
{  
    if(A[i] > A[j])  
        swap(A[i], A[j]);  
    if(i + 1 >= j)  
        return;  
    k = (j - i + 1) / 3;  
    Stooge_Sort(A, i, j-k);  
    Stooge_Sort(A, i+k, j);  
    Stooge_Sort(A, i, j-k);  
}
```
以下内容转http://blog.csdn.net/zhanglei8893

a）对于数组A[i...j]，STOOGE-SORT算法将这个数组划分成均等的3份，分别用A, B, C表示。

第6-8步类似于冒泡排序的思想。它进行了两趟：

第一趟的第6-7步将最大的1/3部分交换到C

第二趟的第8步将除C外的最大的1/3部分交换到B

剩余的1/3位于A，这样的话整个数组A[i...j]就有序了。

b）比较容易写出STOOGE-SORT最坏情况下的运行时间的递归式

T(n) = 2T(2n/3)+Θ(1)

由主定律可以求得T(n)=n^2.71

c）各种排序算法在最坏情况下的运行时间分别为：

插入排序、快速排序：Θ(n^2)

堆排序、合并排序：Θ(nlgn)

相比于经典的排序算法，STOOGE-SORT算法具有非常差的性能，这几位终生教授只能说是浪得虚名了^_^
  

### 7-4 快速排序中的堆栈深度

a)

```c++
void QuickSort2(int *A, int p, int r)
{
	while(p < r)
	{
		int q = Partition(A, int p, r);
		QuickSort2(A, p, q-1);
		p = q + 1;
	}
}
```

b) A = {1, 2, 3, 4, 5, 6}
c)

```c++
void QuickSort3(int *A, int p, int r)
{
	while(p < r)
	{
		int q = Partition(A, int p, r);
		if(r-q > q-p)
		{
			QuickSort3(A, p, q-1);
			p = q + 1;
		}
		else
		{
			QuickSort3(A, q+1, r);
			r = q - 1;
		}
	}
}
```

### 7-5 “三数取中”划分

a)n个数任意取三个不同的数的取法共有C(3,n)种

若要x=A'[i]，必须在A'[1..i-1]中取一个数，在A'[i+1..n]中取一个数取法共(i-1)*(n-i)

```
      (i-1) * (n-i)     6 * (i-1) * (n-i)
pi = --------------- = -------------------
         C(3,n)         n * (n-1) * (n-2)
```

b)在一般实现中，pi=1/n。

n->正无穷时，极限为0。

在这种实现中，当i=(n+1)/2时，

```
      3(n-1)
pi = ---------，当n->正无穷时，极限为0
      2n(n-2)
```

c)遇到这种数学题就没办法了，哎，以前数学没学好

d)不会求

[附自己写的程序](https://github.com/windmissing/exerciseForAlgorithmSecond/blob/master/src/chapter7/Exercise7_5.cpp)
