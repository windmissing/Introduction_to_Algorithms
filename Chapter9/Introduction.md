# 一、概念

第i个顺序统计量是该集合中第i小的元素。  
当n为奇数时，中位数是出现在i=(n+1)/2处的数。当n为偶数时，中位数分别出现在i=n/2和i=(n+1)/2处。  
在本文中，忽略n的奇偶性，中位数是指出现在i=(n+1)/2处的数。  
本文假设集合中的数互异。  

# 二、代码

```c++
#include <iostream>
using namespace std;
//书中的程序
int length_A;
void Print(int *A)
{
	int i;
	for(i = 1; i <= length_A; i++)
		cout<<A[i]<<' ';
	cout<<endl;
}
/***********线性时间求最小值******************************************************/
int Minimun(int *A)
{
	int Min = A[1], i;
	//依次查看集合中的每个元素
	for(i = 2; i < length_A; i++)
		//记录比较过程中最小的元素
		if(Min > A[i])
			Min = A[i];
	return Min;
}
/***********通过3n/2次比较求最小值和最大值******************************************************/
void MinAndMax(int *A,int &Min, int &Max)
{
	int i;
	//如果n是奇数
	if(length_A % 2 == 1)
	{
		//将最大值和最小值设置为第一个元素
		Min = A[1];
		Max = A[1];
		i = 2;
	}
	//如果n是偶数
	else
	{
		//将前两个元素作一次比较，以决定最大值怀最小值的初值
		Min = min(A[1], A[2]);
		Max = A[1] + A[2] - Min;
		i = 3;
	}
	//成对地处理余下的元素
	for(; i <= length_A; i=i+2)
	{
		//将一对输入元素互相比较
		int a = min(A[i], A[i+1]);
		int b = A[i] + A[i+1] - a;
		//把较小者与当前最小值比较
		if(a < Min)
			Min = a;
		//把较大者与当前最大值比较
		if(b > Max)
			Max = b;
	}
}
/**************以期望线性时间作选择**************************************/
//已经出现很多次了，不解释
int Partition(int *A, int p, int r)
{
	int x = A[r], i = p-1, j;
	for(j = p; j < r; j++)
	{
		if(A[j] <= x)
		{
			i++;
			swap(A[i], A[j]);
		}
	}
	swap(A[i+1], A[r]);
	return i+1;
}
 
int Randomized_Partition(int *A, int p, int r)
{
	//随机选择数组中一个数作为主元
	int i = rand() % (r-p+1) + p;
	swap(A[r], A[i]);
	//划分
	return Partition(A, p, r);
}
//i是从1开使计数的，不是从p开始
int Randomized_Select(int *A, int p, int r, int i)
{
	if(p == r)
		return A[p];
	//以某个元素为主元，把数组分为两组，A[p..q-1] < A[q] < A[q+1..r]，返回主元在整个数组中的位置
	int q = Randomized_Partition(A, p, r);
	//主元是整个数组中的第q个元素，是A[p..r]数组中的第k个元素
	int k = q - p + 1;
	//所求的i中A[p..r]中的第i个元素
	if(i == k)//正是所求的元素
		return A[q];
	else if(i < k)//所求元素<主元，则在A[p..q-1]中继续寻找
		return Randomized_Select(A, p, q-1, i);
	else//所求元素>主元，则在A[q+1..r]中继续寻找
		return Randomized_Select(A, q+1, r, i-k);
}
/*************最坏情况线性时间的选择**************************************************/
int Select(int *A, int p, int r, int i);
//对每一组从start到end进行插入排序，并返回中值
//插入排序很简单，不解释
int Insert(int *A, int start, int end, int k)
{
	int i, j;
	for(i = 2; i <= end; i++)
	{
		int t = A[i];
		for(j = i; j >= start; j--)
		{
			if(j == start)
				A[j] = t;
			else if(A[j-1] > t)
				A[j] = A[j-1];
			else
			{
				A[j] = t;
				break;
			}
		}
	}
	return A[start+k-1];
}
//根据文中的算法，找到中值的中值
int Find(int *A, int p, int r)
{
	int i, j = 0;
	int start, end, len = r - p + 1;
	int *B = new int[len/5+1];
	//每5个元素一组，长度为start到end，对每一组进行插入排序，并返回中值
	for(i = 1; i <= len; i++)
	{
		if(i % 5 == 1)
			start = i+p-1;
		if(i % 5 == 0 || i == len)
		{
			j++;
			end = i+p-1;
			//对每一组从start到end进行插入排序，并返回中值,如果是最后一组，组中元素个数可能少于5
			int ret = Insert(A, start, end, (end-start)/2+1);
			//把每一组的中值挑出来形成一个新的数组
			B[j] = ret;	
		}
	}
	//对这个数组以递归调用Select()的方式寻找中值
	int ret = Select(B, 1, j, (j+1)/2);
	//delete []B; //很奇怪，这句话应该是没问题的，但是怎么一运行到这句话就死机呢？
	return ret;
}
//以f为主元的划分
int Partition2(int *A, int p, int r, int f)
{
	int i;
	//找到f的位置并让它与A[r]交换
	for(i = p; i < r; i++)
	{
		if(A[i] == f)
		{
			swap(A[i], A[r]);
			break;
		}
	}
	return Partition(A, p, r);
}
//寻找数组A[p..r]中的第i大的元素，i是从1开始计数，不是从p开始
int Select(int *A, int p, int r, int i)
{
	//如果数组中只有一个元素，则直接返回
	if(p == r)
		return A[p];
	//根据文中的算法，找到中值的中值
	int f = Find(A, p, r);
	//以这个中值为主元的划分，返回中值在整个数组A[1..len]的位置
	//因为主元是数组中的某个元素，划分好是这样的，A[p..q-1] <= f < A[q+1..r]
	int q = Partition2(A, p, r, f);
	//转换为中值在在数组A[p..r]中的位置
	int k = q - p + 1;
	//与所寻找的元素相比较
	if(i == k)
		return A[q];
	else if(i < k)
		return Select(A, p, q-1, i);
	else
		//如果主元是数组中的某个元素，后面一半要这样写
		return Select(A, q+1, r, i-k);
		//但是如果主元不是数组中的个某个元素，后面一半要改成Select(A, q, r, i-k+1)
}
int main()
{
	cin>>length_A;
	int *A = new int[length_A+1], i, cnt;
	//生成测试数据
	for(i = 1; i <= length_A; i++)
		A[i] = rand() % 100;
	cin>>cnt;
	//显示测试数据
	Print(A);
	//输出结果
	if(cnt <= length_A)
		cout<<Select(A, 1, length_A, cnt)<<endl;
	return 0;
}
```