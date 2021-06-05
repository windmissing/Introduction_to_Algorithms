背包问题讲解：
P01: 01背包问题
P02: 完全背包问题
P03: 多重背包问题
P04: 混合三种背包问题
模板代码：Bag.h
#include <iostream>
using namespace std;
#define MAXN 505
#define MAXV 10005
int c[MAXN], v[MAXN], n[MAXN], dp[MAXV];
int N, V;
/*0-1背包
有N件物品和一个容量为V的背包，每种物品都只有一件。
第i件物品的费用是c[i]，价值是w[i]。
f[i][v]表示前i件物品恰放入一个容量为V的背包可以获得的最大价值
f[i][v]=max{f[i-1][v],f[i-1][v-c[i]]+w[i]}*/
void ZeroOnePack(int cost,int value)
{
	int v;
    for( v = V; v >= cost; v--)
        dp[v] = max( dp[v], dp[v-cost]+value);
}

/*完全背包
有N种物品和一个容量为V的背包，每种物品都有无限件可用。
第i种物品的费用是c[i]，价值是w[i]。
令f[i][v]表示前i种物品恰放入一个容量为v的背包的最大权值
f[i][v]=max{f[i-1][v-k*c[i]]+k*w[i]|0<=k*c[i]<=v}
优化：f[i][v]=max{f[i-1][v],f[i][v-c[i]]+w[i]}*/
void CompletePack(int cost, int value)
{
	int v;
    for( v = cost; v <= V; v++)
		dp[v] = max( dp[v], dp[v-cost]+value);
}

/*多重背包
有N种物品和一个容量为V的背包。
第i种物品最多有n[i]件可用，每件费用是c[i]，价值是w[i]。
令f[i][v]表示前i种物品恰放入一个容量为v的背包的最大权值
f[i][v]=max{f[i-1][v-k*c[i]]+k*w[i]|0<=k<=n[i]}*/
void MultiplePack(int cost,int value,int amount)
{
    if( cost * amount >= V)
	{
        CompletePack(cost, value);
        return;
	}
    int k=1;
    while( k < amount)
	{
        ZeroOnePack(k*cost, k*value);
        amount = amount-k;
        k = k * 2;
	}
    ZeroOnePack(amount*cost,amount*value);
}

/*混合三种背包
for i=1..N
    if 第i件物品属于01背包
        ZeroOnePack(c[i],w[i])
    else if 第i件物品属于完全背包
        CompletePack(c[i],w[i])
    else if 第i件物品属于多重背包
        MultiplePack(c[i],w[i],n[i])
*/


