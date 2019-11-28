---
layout: post
title: " 算法导论-15-2-整齐打印"
category: 算法导论
tags: [算法, 算法导论, 动态规划]
---

题目：
![](http://blog.csdn.net/mishifangxiangdefeng/article/details/7921947)  

思考：
定义
（1）extra为行末多余空格字符个数的立方和
（2）f[i,j] = M - j + i - SUM(lk) , i<=k<=j
（3）令len[i,j]表示第i个单词到第j个单词可以得到的最小extra
整齐打印问题可以分解成以下子问题
（1）若f[i,j] < 0,，则len[i,j] = MIN(len[i,k] + len[k+1,j]) , i<=k<j
（2）若f[i,j] > 0 && j==n，则len[i,j] = 0
（3）若i==j，则len[i,j] = (f[i,j])^3
代码：为了编程方便，程序和说明略有不同
#include <iostream>
#include <cmath>
using namespace std;

#define M 10//一行的最大长度
#define N 10//单词的个数

int s[N][N];

void DP(int *len)
{
	int i, j, k, step, temp;
	for(step = 0; step < N; step++)
	{
		for(i = 0; i < N; i++)
		{
			temp = 0;j = i + step;
			if(j >= N)
				break;
			//计算len[i,j]
			for(k = i; k <= j; k++)
			{
				if(k != i) temp++;
				temp = temp + len[k];
				if(temp > M)
					break;
			}
			if(temp > M)s[i][j] = 0x7fffffff;
			//若f[i,j] > 0 && j==n，则len[i,j] = 0
			else if(j == N-1)s[i][j] = 0;
			//若i==j，则len[i,j] = (f[i,j])^3
			else s[i][j] = pow(M*1.0-temp,3);
			//若f[i,j] < 0,，则len[i,j] = MIN(len[i,k] + len[k+1,j]) , i<=k<j
			for(k = i; k < j; k++)
				if(s[i][k]+s[k+1][j] < s[i][j])
					s[i][j] = s[i][k]+s[k+1][j];
		}
	}
	cout<<s[0][N-1]<<endl;
}
void Print()
{
	int i, j;
	for(i = 0; i < N; i++)
	{
		for(j = 0; j < N; j++)
			cout<<s[i][j]<<' ';
		cout<<endl;
	}
}
/*1 2 3 1 2 3 1 2 3 1*/
int main()
{
	int len[N], i;
	//输出数据
	for(i = 0; i < N; i++)
		cin>>len[i];
	DP(len);
//	Print();
	return 0;
}

 