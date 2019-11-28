15.1装配线调度

15.1-1

//15.1-1
void Print_Stations2(int i, int j)
{
	cout<<"顺序输出装配路线"<<endl;
	if(j != n)
		i = l[i][j+1];
	//先输出前面的
	if(j > 1)
		Print_Stations2(i, j-1);
	//再输出当前的
	cout<<"line "<<i<<" station "<<j<<endl;
}


15.1-4

不使用l数组来记录，输出时根据类似L4-L13的比较来计算出前一个station的数据
 
15.2矩阵链乘法

15.2-1

算法：类似于15.3的做备忘录
运行结果：((A1A2)((A3A4)(A5A6)))

15.2-2

//15.2-2递归算法
int Matrix_Chain_Order(int *A, int start, int end)
{
	//只有一个矩阵时，不需要括号
	if(start == end)
		return 0;
	//如果已经有结果，直接使用结果
	if(m[start][end])
		return m[start][end];
	int i, q;
	m[start][end] = 0x7fffffff;
	//P199，公式15.15
	for(i = start; i < end; i++)
	{
		q = Matrix_Chain_Order(A, start, i) + 
			Matrix_Chain_Order(A, i+1, end) + 
			A[start-1] * A[i] * A[end];
		//选最小值
		if(q < m[start][end])
		{
			m[start][end] = q;
			s[start][end] = i;
		}
	}
	return m[start][end];
}

 
15.3动态规划基础

15.3-1

枚举的时间的复杂度是O(4^n)/(n^(3/2))
RECURSIVE-MATRIX-CHAIN的时间复杂度是O(n*3^(n-1))
显然后者更有效
 
15.3-3

解释见1楼
#include <iostream>
using namespace std;

#define N 6
int m[N+1][N+1] = {0}, s[N+1][N+1] = {0};
//15.2-2递归算法
int Matrix_Chain_Order(int *A, int start, int end)
{
	//只有一个矩阵时，不需要括号
	if(start == end)
		return 0;
	//如果已经有结果，直接使用结果
	if(m[start][end])
		return m[start][end];
	int i, q;
	m[start][end] = -1;
	//P199，公式15.15
	for(i = start; i < end; i++)
	{
		q = Matrix_Chain_Order(A, start, i) + 
			Matrix_Chain_Order(A, i+1, end) + 
			A[start-1] * A[i] * A[end];
		//选最小值
		if(q > m[start][end])
		{
			m[start][end] = q;
			s[start][end] = i;
		}
	}
	return m[start][end];
}
//输出结果
void Print_Optimal_Parens(int *A, int i, int j)
{
	if(i == j)
		cout<<'A'<<i;
	else
	{
		cout<<'(';
		Print_Optimal_Parens(A, i, s[i][j]);
		Print_Optimal_Parens(A, s[i][j]+1, j);
		cout<<")";
	}
}
int main()
{
	int A[N+1] = {5, 10, 3, 12, 5, 50, 6};
	int A2[N+1] = {30, 35, 15, 5, 10, 20, 25};
	Matrix_Chain_Order(A, 1, N);
	Print_Optimal_Parens(A, 1, N);
	return 0;
}


 15.4最长公共子序列

15.4-1

1 0 0 1 1 0
15.4-2

//15.4-2 不使用表b的情况下计算最LCS并输出
void Lcs_Length2(int *x, int *y)
{
	int i, j;
	//初始化
	for(i = 1; i <= M; i++)
		c[i][0] = 0;
	for(j = 1; j <= N; j++)
		c[0][j] = 0;
	//求LCS的时间没有什么区别，只要把与b有关的去掉就可以了
	for(i = 1; i <= M; i++)
	{
		for(j = 1; j <= N; j++)
		{
			//第一种情况
			if(x[i] == y[j])
				c[i][j] = c[i-1][j-1] + 1;
			else
			{
				//第二种情况
				if(c[i-1][j] >= c[i][j-1])
					c[i][j] = c[i-1][j];
				//第三种情况
				else
					c[i][j] = c[i][j-1];
			}
		}
	}
}
//区别在于输出，根据计算反推出前一个数据，而不是通过查找获得
void Print_Lcs2(int *x, int i, int j)
{
	//递归到初始位置了
	if(i == 0 || j == 0)
		return;
	//三种情况，刚好与Lcs_Length2中的三种情况相对应（不是按顺序对应）
	//第二种情况
	if(c[i][j] == c[i-1][j])
		Print_Lcs2(x, i-1, j);
	//第三种情况
	else if(c[i][j] == c[i][j-1])
		Print_Lcs2(x, i, j-1);
	//第一种情况
	else
	{
		//匹配位置
		Print_Lcs2(x, i-1, j-1);
		cout<<x[i]<<' ';
	}
}

15.4-3

//15.4-3备忘录版本，类似于递归，只是对做过的计算记录下来，不重复计算
//每一次迭代是x[1..m]和y[1..n]的匹配
int Lcs_Length3(int *x, int *y, int m, int n)
{
	//长度为0，肯定匹配为0
	if(m == 0|| n == 0)
		return 0;
	//若已经计算，直接返回结果
	if(c[m][n] != 0)
		return c[m][n];
	//公式15.14的对应
	if(x[m] == y[n])
		c[m][n] = Lcs_Length3(x, y, m-1, n-1) + 1;
	else
	{
		int a = Lcs_Length3(x, y, m-1, n);
		int b = Lcs_Length3(x, y, m, n-1);
		c[m][n] = a > b ? a : b;
	}
	return c[m][n];
}

15.4-4

（1）使用2*min(m,n)及O(1)的额外空间来计算LCS的长度
因为这里只需要求长度，而不需要求序列，可以只存储需要的内容。每一次的计算c[i][j]只与c[i-1][j]、c[i][j-1]、c[i-1][j-1]有关，所以只保留第i行和第i-1行
//15.4-4(1)使用2*min(m,n)及O(1)的额外空间来计算LCS的长度
void Lcs_Length4(int *x, int *y)
{
	int i, j;
	//c2是2*min(M,N)的矩阵，初始化
	memset(c2, 0 ,sizeof(c2));
	//类似于上文的循环，只是i%2代表当前行，(i-1)%2代表上一行，其余内容相似
	for(i = 1; i <= N; i++)
	{
		for(j = 1; j <= M; j++)
		{
			if(y[i] == x[j])
				c2[i%2][j] = c2[(i-1)%2][j-1] + 1;
			else
			{
				if(c2[(i-1)%2][j] >= c2[i%2][j-1])
					c2[i%2][j] = c2[(i-1)%2][j];
				else
					c2[i%2][j] = c2[i%2][j-1];
			}
		}
	}
	//输出结果
	cout<<c2[N%2][M]<<endl;
}

（2）使用min(m,n)项以及O(1)空间
//15.4-4(2)使用min(m,n)及O(1)的额外空间来计算LCS的长度
void Lcs_Length4(int *x, int *y)
{
	int i, j;
	//c2是min(M,N)的矩阵，初始化
	memset(c2, 0 ,sizeof(c2));
	//类似于上文的循环，只是i%2代表当前行，(i-1)%2代表上一行，其余内容相似
	int t1 = 0, t2;
	for(i = 1; i <= N; i++)
	{
		t2 = c[j];
		for(j = 1; j <= M; j++)
		{
			if(y[i] == x[j])
				c2[j] = t1 + 1;
			else
				c2[j] = max(c2[j], c2[j-1]);
			t1 = t2;
		}
	}
	//输出结果
	cout<<c2[M]<<endl;
}

 
15.4-5

#include <iostream>
#include <string>
using namespace std;

#define N 10
int c[N+1] = {0};//c[i]表示A[1..i]的最长递增子序列
int pre[N+1];//pre[i]表示若要得到A[1..i]的最长递增子序列，i的前一个是哪个
//求最长递增子序列
void Length(int *A)
{
	int i, j;
	//A[i] = max{A[j]+1} if A[j]>A[i] and j<i
	for(i = 1; i <= N; i++)
	{
		//初始化
		c[i] = 0;
		pre[i] = 0;
		for(j = 0; j < i; j++)
		{
			if(A[i] > A[j])
			{
				if(c[j] + 1 > c[i])
				{
					c[i] = c[j]+1;
					pre[i] = j;
				}
			}
		}
	}
	cout<<c[N]<<endl;
}
//若要输出A[1..n]中的最长单调子序列，先输出A[1..pre[n]]中的最长单调子序列
void Print(int *A, int n)
{
	if(pre[n])
		Print(A, pre[n]);
	cout<<A[n]<<' ';
}
int main()
{
	//因为从第开始记数，所以数组中的第一个数不算，只是占一个位置
//	int A[N+1] = {0,1,2,3,4,5,6,7,8,9,10};
	int A[N+1] = {0,11,2,13,4,15,6,17,8,19,10};
	Length(A);
	Print(A, N);
	return 0;
}

 
15.1最优二叉查找树

15.5-1

//15.5-1输出最优二叉查找树
void Construct_Optimal_Best(int start, int end)
{
	//找到根结点
	int r = root[start][end];
	//如果左子树是叶子
	if(r-1 < start)
		cout<<'d'<<r-1<<" is k"<<r<<"'s left child"<<endl;
	//如果左子树不是叶子
	else
	{
		cout<<'k'<<root[start][r-1]<<" is k"<<r<<"'s left child"<<endl;
		//对左子树递归使用Construct_Optimal_Best
		Construct_Optimal_Best(start, r-1);
	}
	//如果右子树是叶子
	if(end < r+1)
		cout<<'d'<<end<<" is k"<<r<<"'s right child"<<endl;
	//如果右子树不是叶子
	else
	{
		cout<<'k'<<root[r+1][end]<<" is k"<<r<<"'s right child"<<endl;
		//对右子树递归使用Construct_Optimal_Best
		Construct_Optimal_Best(r+1, end);
	}
}

15.5-2

k5 is root
k2 is k5's left child
k1 is k2's left child
d0 is k1's left child
d1 is k1's right child
k3 is k2's right child
d2 is k3's left child
k4 is k3's right child
d3 is k4's left child
d4 is k4's right child
k6 is k5's right child
d5 is k6's left child
k7 is k6's right child
d6 is k7's left child
d7 is k7's right child
 
15.5-4

//15.5-4
void Optimal_Bst(double * p, double *q, int n)
{
	int i, j, r ,l;
	double t;
	//初始化。当j=i-1时，只有一个虚拟键d|i-1
	for(i = 1; i <= n+1; i++)
	{
		e[i][i-1] = q[i-1];
		w[i][i-1] = q[i-1];
	}
	//公式15.19
	for(l = 1; l <= n; l++)
	{
		for(i = 1; i <= n-l+1; i++)
		{
			j = i+l-1;
			e[i][j] = 0x7fffffff;
			//公式15.20
			w[i][j] = w[i][j-1] + p[j] + q[j];
			//root[i,j-1] <= root[i,j] <= root[i+1,j]
			for(r = root[i][j-1]; r <= root[i+1][j]; r++)
			{
				//公式15.19
				t = e[i][r-1] + e[r+1][j] + w[i][j];
				//取最小值
				if(t < e[i][j])
				{
					e[i][j] = t;
					//记录根结点
					root[i][j] = r;
				}
			}
		}
	}
}
