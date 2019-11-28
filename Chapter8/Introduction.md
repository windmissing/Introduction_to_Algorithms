# 一、概念

1. 比较排序

比较排序是指通过输入元素间的比较来确定各元素次序的排序算法。  
任何比较排序在最坏情况下都要用O(nlgn)次比较来进行排序  

合并排序和堆排序是渐近最优的



2. 非比较排序

非比较排序指使用一些非比较的操作来确定排序顺序的排序算法

对于非比较排序，下界O(nlgn)不适用

计数排序是稳定排序，若n个数据的取值范围是[0..k]，则运行时间为O(n+k)，运行空间是O(n+k)

基数排序也是稳定排序，需要另一个稳定排序作为基础，若n个d位数，每一位有k种取值可能，所用的稳定排序运行时间为O(n+k)，则基数排序的时间是O(d(n+k))

桶排序也是稳定排序，当输入数据符合均匀分布时，桶排序可以以线性时间运行。所设所有元素均匀分布在区间[0,1)上，把区间[0,1)划分成n个相同大小的子区间（桶），对各个桶中的数进行排序，把依次把各桶中的元素列出来。

 
# 二、代码

```c++
#include <iostream>
#include <cmath>
using namespace std;
 
int length_A, digit;
 
void Print(int *A, int start, int end)
{
	int i;
	for(i = start; i <= end; i++)
	{
		if(i == start)cout<<'{';
		else cout<<' ';
		cout<<A[i];
	}
	cout<<'}'<<endl;
}
 
//计数排序
void Counting_Sort(int *A, int *B, int k)
{
	int i, j;
	//将C数组初始化为0，用于计数
	int *C = new int[k+1];
	for(i = 0; i <= k; i++)
		C[i] = 0;
	//C[j]表示数字j在数组A中出现的次数
	for(j = 1; j <= length_A; j++)
		C[A[j]]++;
	//C[i]表示所以<=i的数字出现过的次数
	for(i = 1; i <= k; i++)
		C[i] = C[i] + C[i-1];
	//初始化B为0，B用于输出排序结果
	for(i = 1; i <= length_A; i++)
		B[i] = 0;
	for(j = length_A; j >= 1; j--)
	{
		//如果<=A[j]的数字的个数是x,那么排序后A[j]应该出现在第x个位置，即B[x]=A[j]
		B[C[A[j]]] = A[j];
		C[A[j]]--;
	}
	delete C;
}
//基数排序调用的稳定排序
void Stable_Sort(int *A, int *B, int k, int d)
{
	int i, j;
	//将C数组初始化为0，用于计数
	int *C = new int[k+1];
	for(i = 0; i <= k; i++)
		C[i] = 0;
	int *D = new int[length_A+1];
	for(j = 1; j <= length_A; j++)
	{
		//D[j]表示第[j]个元素的第i位数字
		D[j] = A[j] % (int)pow(10.0, d) / (int)pow(10.0, d-1);
		//C[j]表示数字D[j]在数组A中出现的次数
		C[D[j]]++;
	}
	//C[i]表示所以<=i的数字出现过的次数
	for(i = 1; i <= k; i++)
		C[i] = C[i] + C[i-1];
	//初始化B为0，B用于输出排序结果
	for(i = 1; i <= length_A; i++)
		B[i] = 0;
	for(j = length_A; j >= 1; j--)
	{
		//如果<=D[j]的数字的个数是x,那么排序后A[j]应该出现在第x个位置，即B[x]=A[j]
		B[C[D[j]]] = A[j];
		C[D[j]]--;
	}
	delete []C;
	delete []D;
}
//基数排序
void Radix_Sort(int *A, int *B)
{
	int i, j;
	//依次对每一位进行排序，从低位到高位
	for(i = 1; i <= digit; i++)
	{
		Stable_Sort(A, B, 9, i);
		//输入的是A，输出的是B，再次排序时要把输出数据放入输出数据中
		for(j = 1; j <= length_A; j++)
		A[j] = B[j];
	}
}
 
int main()
{
	cin>>length_A>>digit;
	int *A = new int[length_A+1];
	int *B = new int[length_A+1];
	int i;
	//随机产生length_A个digit位的数据
	for(i = 1; i <= length_A; i++)
	{
		A[i] = 0;
		while(A[i] < (int)pow(10.0, digit-1))
			A[i] = rand() % (int)pow(10.0, digit);
	}
	Print(A, 1, length_A);
//	Counting_Sort(A, B, 9);
	Radix_Sort(A, B);
	Print(A, 1, length_A);
	delete []A;
	delete []B;
	return 0;
}
```