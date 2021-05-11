一、介绍
二、代码
#include <iostream>
#include <algorithm>
using namespace std;

#define N 6//点的个数
#define M 10//边的个数
//邻接矩阵
struct Graph
{
	int map[N+1][N+1];
	int row;
	Graph(int n):row(n)
	{
		int i, j;
		for(i = 1; i <= N; i++)
		{
			for(j = 1; j <= N; j++)
				if(i == j)
					map[i][j] = 0;
				else
					map[i][j] = 0x7fffffff;
		}
	}
	Graph(Graph &G)
	{
		row = G.row;
		int i, j;
		for(i = 1; i <= N; i++)
		{
			for(j = 1; j <= N; j++)
				map[i][j] = G.map[i][j];
		}
	}
};
int min(int a, int b)
{
	return a < b ? a : b;
}
void Print(Graph G)
{
	int i, j;
	for(i = 1; i <= N; i++)
	{
		for(j = 1; j <= N; j++)
			cout<<G.map[i][j]<<' ';
		cout<<endl;
	}
	cout<<endl;
}
Graph Extend_Shortest_Paths(Graph L, Graph W)
{
	int i, j, k, n = W.row;
	Graph ret(n);
	for(i = 1; i <= n; i++)
	{
		for(j = 1; j <= n; j++)
		{
			ret.map[i][j] = 0x7fffffff;
			for(k = 1; k <= n; k++)
			{
				if(L.map[i][k] != 0x7fffffff && W.map[k][j] != 0x7fffffff)
					ret.map[i][j] = min(ret.map[i][j], L.map[i][k] + W.map[k][j]);
			}
		}
	}
	return ret;
}
Graph Matrix_Multiply(Graph A, Graph B)
{
	int i, j, k, n = A.row;
	Graph ret(n);
	for(i = 1; i <= n; i++)
	{
		for(j = 1; j <= n; j++)
		{
			ret.map[i][j] = 0;
			for(k = 1; k <= n; k++)
			{
				ret.map[i][j] = ret.map[i][j] + A.map[i][k] * B.map[k][j];
			}
		}
	}
	return ret;
}
Graph Slow_All_Pairs_Shortest_Paths(Graph W)
{
	int n = W.row, m;
	Graph L(W);
	for(m = 2; m < n; m++)
	{
		L = Extend_Shortest_Paths(L, W);
//		Print(L);
	}
	return L;
}
Graph Faster_All_Pairs_Shortest_Paths(Graph W)
{
	int n = W.row, m = 1;
	Graph L(W);
	while(m < n - 1)
	{
		L = Extend_Shortest_Paths(L, L);
		m = 2 * m;
		Print(L);
	}
	return L;
}
/*
1 5 -1
2 1 1
2 4 2
3 2 2
3 6 -8
4 1 -4
4 5 3
5 2 7
6 2 5
6 3 10
*/
int main()
{
	int i, start, end, value;
	Graph G(N);
	for(i = 1;i <= M; i++)
	{
		cin>>start>>end>>value;
		G.map[start][end] = value;
	}
	Print(G);
	Slow_All_Pairs_Shortest_Paths(G);
	Faster_All_Pairs_Shortest_Paths(G);
	return 0;
}


[算法导论-25.2-Floyd-Wasrshall算法](http://blog.csdn.net/mishifangxiangdefeng/article/details/7867513)  