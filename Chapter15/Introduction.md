一、综述

动态规划是通过组合子问题的解而解决整个问题的。
动态规划适用于子问题不是独立的情况，也就是各子问题的包含公共的子子问题。
动态规划对每个子问题只求解一次，将其结果保存在一张表中。
动态规划通常用于最优化问题。
动态规划的设计步骤：a.描述最优解的结构b.递归定义最优解的值c.按自底向上的方式计算最优觖的值d.由计算出的结构构造一个最优解

二、代码

15.1装配线调度

#include <iostream>
using namespace std;

#define I 2
#define J 6
int a[I+1][J+1], t[I+1][J+1], e[I+1], x[I+1], n = J;//输入
int f[I+1][J+1], l[I+1][J+1],rf,rl;                 //输出
//书上的伪代码
void Fastest_Way()
{
	//初始化
	f[1][1] = e[1] + a[1][1];
	f[2][1] = e[2] + a[2][1];
	int j;
	for(j = 2; j <= n; j++)
	{
		//根据公式15.6计算
		if(f[1][j-1] + a[1][j] <= f[2][j-1] + t[2][j-1] + a[1][j])
		{
			//记录计算结果
			f[1][j] = f[1][j-1] + a[1][j];		
			l[1][j] = 1;
		}
		else
		{
			//记录计算结果
			f[1][j] = f[2][j-1] + t[2][j-1] + a[1][j];
			l[1][j] = 2;
		}
		//根据公式15.7计算
		if(f[2][j-1] + a[2][j] <= f[1][j-1] + t[1][j-1] + a[2][j])
		{
			//记录计算结果
			f[2][j] = f[2][j-1] + a[2][j];
			l[2][j] = 2;
		}
		else
		{
			//记录计算结果
			f[2][j] = f[1][j-1] + t[1][j-1] + a[2][j];
			l[2][j] = 1;
		}
	}
	//最后一个结果另外记录
	if(f[1][n] + x[1] <= f[2][n] + x[2])
	{
		rf = f[1][n] + x[1];
		rl = 1;
	}
	else
	{
		rf = f[2][n] + x[2];
		rl = 2;
	}
}
//输出
void Print_Stations()
{
	cout<<"输出装配路线"<<endl;
	int i = rl, j;
	//从后向前输出
	cout<<"line "<<i<<" station "<<n<<endl;
	for(j = n; j > 1; j--)
	{
		//获取记录的结果
		i = l[i][j];
		cout<<"line "<<i<<" station "<<j-1<<endl;
	}
}
//练习
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
//输入数据
void Input()
{
	int i, j;
	cout<<"输入在装配站S(i,j)的装配时间a(i,j)："<<endl;
	//7 9 3 4 8 4 8 5 6 4 5 7
	for(i = 1; i <= I; i++)
	{
		for(j = 1; j <= J; j++)
			cin>>a[i][j];
	}
	cout<<"输入进入装配线i的花费时间e(i)："<<endl;
	//2 4
	for(i = 1; i <= I; i++)
		cin>>e[i];
	cout<<"输入从S(i,j)站移动S(i2,j+1)的花费时间t(i,j)："<<endl;
	//2 3 1 3 4 2 1 2 2 1
	for(i = 1; i <= I; i++)
	{
		for(j = 1; j < J; j++)
			cin>>t[i][j];
	}
	cout<<"输入从装配线i离开的花费时间x(i)："<<endl;
	//3 2
	for(i = 1; i <= I; i++)
		cin>>x[i];
}
void Output()
{
	int i, j;
	cout<<"输出f[i][j]"<<endl;
	//f[i][j]表示第j个站是在装配线i上完成的，完成1到j的装配最少需要的时间
	for(i = 1; i <= I; i++)
	{
		for(j = 1; j <= J; j++)
			cout<<f[i][j]<<' ';
		cout<<endl;
	}
	cout<<"输出l[i][j]"<<endl;
	//l[i][j]表示使得f[i][j]最小时在哪个装配线上装配j-1
	for(i = 2; i <= I; i++)
	{
		for(j = 1; j <= J; j++)
			cout<<l[i][j]<<' ';
		cout<<endl;
	}
}
int main()
{
	Input();
	Output();
	Fastest_Way();
	Print_Stations();
	Print_Stations2(rl, J);
	return 0;
}


15.2矩阵链乘法

#include <iostream>
using namespace std;

#define N 6
int m[N+1][N+1] = {0}, s[N+1][N+1] = {0};
void Matrix_Chain_Order(int *p)
{
	int i, l, j, k, q;
	for(i = 1; i <= N; i++)
		m[i][i] = 0;
	for(l = 2; l <= N; l++)
	{
		for(i = 1; i <= N - l + 1; i++)
		{
			j = i + l - 1;
			m[i][j] = 0x7fffffff;
			for(k = i; k <= j - 1; k++)
			{
				q = m[i][k] + m[k+1][j] + p[i-1]*p[k]*p[j];
				if(q < m[i][j])
				{
					m[i][j] = q;
					s[i][j] = k;
				}
			}
		}
	}
}
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

15.3动态规划基础

#include <iostream>
using namespace std;

#define N 6
int m[N+1][N+1] = {0}, s[N+1][N+1] = {0};
//重叠子问题
int Recursive_Matrix_Chain(int *p, int i, int j)
{
	//只有一个矩阵时，不需要括号
	if(i == j)
		return 0;
	//如果已经有结果，直接使用结果
	if(m[i][j])
		return m[i][j];
	int k, q;
	m[i][j] = 0x7fffffff;
	//P199，公式15.15
	for(k = i; k < j; k++)
	{
		q = Recursive_Matrix_Chain(p, i, k) + 
			Recursive_Matrix_Chain(p, k+1, j) + 
			p[i-1] * p[k] * p[j];
		//选最小值
		if(q < m[i][j])
		{
			m[i][j] = q;
			s[i][j] = k;
		}
	}
	return m[i][j];
}
//做备忘录
int Lookup_Chain(int *p, int i, int j)
{
	int k, q;
	if(m[i][j] < 0x7fffffff)
		return m[i][j];
	if(i == j)
		m[i][j] = 0;
	else
	{
		for(k = i; k < j; k++)
		{
			q = Lookup_Chain(p, i, k) + Lookup_Chain(p, k+1, j) + p[i-1]*p[k]*p[j];
			if(q < m[i][j])
			{
				m[i][j] = q;
				s[i][j] = k;
			}
		}
	}
	return m[i][j];
}
int Medorized_Matrix_Chain(int *p)
{
	int n = N, i, j;
	for(i = 1; i <= n; i++)
	{
		for(j = i; j <= n; j++)
			m[i][j] = 0x7fffffff;
	}
	return Lookup_Chain(p, 1, n);
}
//输出结果
void Print_Optimal_Parens(int *p, int i, int j)
{
	if(i == j)
		cout<<'A'<<i;
	else
	{
		cout<<'(';
		Print_Optimal_Parens(p, i, s[i][j]);
		Print_Optimal_Parens(p, s[i][j]+1, j);
		cout<<")";
	}
}
int main()
{
	int A[N+1] = {5, 10, 3, 12, 5, 50, 6};
	int A2[N+1] = {30, 35, 15, 5, 10, 20, 25};
	//重叠子问题
	memset(s, 0, sizeof(s));
	cout<<Recursive_Matrix_Chain(A, 1, N)<<endl;
	Print_Optimal_Parens(A, 1, N);
	cout<<endl;
	//做备忘录
	memset(s, 0, sizeof(s));
	cout<<Medorized_Matrix_Chain(A)<<endl;
	Print_Optimal_Parens(A, 1, N);
	cout<<endl;
	return 0;
}


15.4最长公共子序列

#include <iostream>
using namespace std;

#define M 8
#define N 9

int b[M+1][N+1] = {0}, c[M+1][N+1] = {0};
int c2[2][M+1] = {0};
/********书上的伪代码*******************************************/
void Lcs_Length(int *x, int *y)
{
	int i, j;
	//初始化
	for(i = 1; i <= M; i++)
		c[i][0] = 0;
	for(j = 1; j <= N; j++)
		c[0][j] = 0;
	//根据公式15.14计算
	for(i = 1; i <= M; i++)
	{
		for(j = 1; j <= N; j++)
		{
			//记录计算结果
			if(x[i] == y[j])
			{
				c[i][j] = c[i-1][j-1] + 1;
				b[i][j] = 2;
			}
			else
			{
				if(c[i-1][j] >= c[i][j-1])
				{
					c[i][j] = c[i-1][j];
					b[i][j] = 1;
				}
				else
				{
					c[i][j] = c[i][j-1];
					b[i][j] = 3;
				}
			}
		}
	}
}
//输出
void Print_Lcs(int *x, int i, int j)
{
	if(i == 0 || j == 0)
		return;
	if(b[i][j] == 2)
	{
		Print_Lcs(x, i-1, j-1);
		cout<<x[i]<<' ';
	}
	else if(b[i][j] == 1)
		Print_Lcs(x, i-1, j);
	else
		Print_Lcs(x, i, j-1);
}
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
void Lcs_Length5(int *x, int *y)
{
	int i, j, temp = 0;
	memset(c2, 0 ,sizeof(c2));
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
	cout<<c2[N%2][M]<<endl;
}
void Print()
{
	int i, j;
	for(i = 1; i <= M; i++)
	{
		for(j = 1; j <= N; j++)
			cout<<c[i][j]<<' ';
		cout<<endl;
	}
}
int main()
{
	int x[M+1] = {0,1,0,0,1,0,1,0,1};
	int y[N+1] = {0,0,1,0,1,1,0,1,1,0};
	Lcs_Length(x, y);
//	Print();
	Print_Lcs(x, M, N);
//	Lcs_Length2(x, y);
//	Lcs_Length3(x, y, M, N);
//	Print();
//	Print_Lcs2(x, M, N);
//	Lcs_Length4(x, y);
	return 0;
}

15.5最优二叉查找树

#include <iostream>
using namespace std;

#define N 7

double e[N+2][N+2] = {0};//期望
double w[N+2][N+2] = {0};//概率
int root[N+2][N+2] = {0};//记录树的根结点的位置
/*********调试过程中用于输出中间信息***************************************************/
void PrintE()
{
	int i, j;
	for(i = 1; i <= N+1; i++)
	{
		for(j = 1; j <= N+1; j++)
			cout<<e[i][j]<<' ';
		cout<<endl;
	}
	cout<<endl;
}
void PrintW()
{
	int i, j;
	for(i = 1; i <= N+1; i++)
	{
		for(j = 1; j <= N+1; j++)
			cout<<w[i][j]<<' ';
		cout<<endl;
	}
	cout<<endl;
}
void PrintRoot()
{
	int i, j;
	for(i = 1; i <= N+1; i++)
	{
		for(j = 1; j <= N+1; j++)
			cout<<root[i][j]<<' ';
		cout<<endl;
	}
	cout<<endl;
}
/********书上的伪代码对应的程序******************************************************/
//构造最做树
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
			for(r = i; r <= j; r++)
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
/********练习********************************************/
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
int main()
{
	int n = N;
//	double p[N+1] = {0, 0.15, 0.10, 0.05, 0.10, 0.20};
//	double q[N+1] = {0.05, 0.10, 0.05, 0.05, 0.05, 0.10};
	double p[N+1] = {0, 0.04, 0.06, 0.08, 0.02, 0.10, 0.12, 0.14};
	double q[N+1] = {0.06, 0.06, 0.06, 0.06, 0.05, 0.05, 0.05};
	Optimal_Bst(p, q, n);
//	PrintE();
//	PrintW();
//	PrintRoot();
	cout<<'k'<<root[1][N]<<" is root"<<endl;
	Construct_Optimal_Best(1, N);
	return 0;
}

 
 [ 第15章 动态规划](http://blog.csdn.net/mishifangxiangdefeng/article/details/7744512) 