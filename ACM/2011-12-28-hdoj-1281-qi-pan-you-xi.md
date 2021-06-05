#### 题目链接

http://acm.hdu.edu.cn/showproblem.php?pid=1281

二分匹配, 增广链

#### 题目描述

在一个N*M大小的棋盘中，有K个空位置，

（1）在这些空位置上最多能放多少的“车”（一行或一列最多一个）。

（2）空位置中，有的位置若不放“车”，就无法保证放尽量多的“车”，这样的格子被称做重要点，求重要点的个数

#### 思考过程

这题可以看成行与列的二分匹配问题，因为每行每列至多只能放一个棋子。第i行与j列匹配代表棋盘第ｉ行ｊ列这个位置放棋子。

那么，棋盘上的点就是二分图的边；“车”的个数就是二分图的最大匹配数。

题目的关键是求重要点。

现假设最大匹配数为ans，且已经求出某一种匹配策略。

1 ：枚举所有可以放的点，去掉某一点后（这里的点指棋盘上的点，也就是二分图的边），就得到一个新的二分图了
　　 if  (新二分图的最大匹配数　=＝　ans）
                 then 这个点不是重要点
　　 else // 即新的二分图达不到ans这个匹配数，那么这个点就是必须放的，否则达不到ans。 -->重要边
　　          then 计数+1

2 : 但是这样枚举效率太低。实际上，删边只需考虑求出的匹配边（因为删除非匹配边得到的匹配数不变）。这样，只需删除ans条边，复杂度就降低了。

再进一步分析，删除一条边以后，没有必要重新求删边后新的二分图的最大匹配，只需检查删边后的匹配中--->可不可以再找到新的增广链就可以了。这样，时间复杂度就进一步降到了。

3 : 这样的优化是不可取的

在判断是否存在增广路得时候，不能只以删除的匹配边的顶点作起点来找增广路

正确的方法是：以删边后新的二分图的所有未匹配顶点出发做增广，都找不到增广路，匹配不能再增加

#### 代码

```c++
#include <iostream>
using namespace std;

const int MAXN = 105;
int N, M;   
bool map[MAXN][MAXN];
int xM[MAXN], yM[MAXN]; 
bool chk[MAXN];

bool SearchPath(int &u,bool &flag);
int MaxMatch();
bool Can();

int main()
{
    int i,a,b,K,ans,Case = 0;
	while(cin>>N>>M>>K)
    {
		//初始化
        N--;M--;Case++;
        memset(map,0,sizeof(map));
        for (i = 0; i < K; i++)
        {
			cin>>a>>b;
            map[--a][--b] = 1;
        }
		//二分匹配，求最大匹配数
        ans = MaxMatch();    
        int tmp,num = 0;
        for (int u = 0; u <= N; u++)
        {
			//是一条匹配边
            if (xM[u] != -1)
            {
				//删除这条匹配边
                tmp = xM[u];
                xM[u] = -1;
                yM[tmp] = -1;
                map[u][tmp] = 0;
				//是否找到新的增广路
                if (!Can())num++;
				//恢复这条边
                xM[u] = tmp;
                yM[tmp] = u;
                map[u][tmp] = 1;
            }
        }
		//输出结果
		cout<<"Board "<<Case<<" have "<<num<<" important blanks for "<<ans<<" chessmen."<<endl;
	}
    return 0;
}
//二分匹配，求最大匹配数，这个过程类似于DFS
int MaxMatch()
{
	//初始化
    int u, ret = 0 ;
    bool flag = true;
    memset(xM, -1, sizeof (xM));
    memset(yM, -1, sizeof (yM));
    //对每个未匹配的顶点进行尝试匹配
    for(u = 0; u <= N; u++)
    {
		//若顶点还没有找到匹配
        if(xM[u] == -1)
        {
			//初始化
            memset(chk, false, sizeof (chk));
			//尝试匹配，若成功，计数+1
            if(SearchPath(u,flag)) ret++;
        }
    }
    return ret;
}
//尝试匹配
bool SearchPath(int &u,bool &flag)
{
    int v;
    for(v = 0; v <= M; v++)
    {
        if(map[u][v] && !chk[v])
        {
            chk[v] = true;
            if(yM[v] == -1 || SearchPath(yM[v],flag))
            {
                if (flag)
                {
                    yM[v] = u; xM[u] = v;
                }
                return true ;
            }
        }
    }
    return false ;
}
//是否找到新的增广路
bool Can()
{
    int u;
    bool flag = false;  
	//以对所有未匹配的顶点进行尝试匹配
    for(u = 0; u <= N; u++)
    {
        if(xM[u] == -1)
        {
            memset(chk, false, sizeof (chk));
			//找到一条增广路
            if(SearchPath(u,flag)) 
                return true;
        }
    }
    return false;
}
```

#### 总结：

复习了一下二分匹配算法

求二分匹配增广路的方法很巧妙

二分匹配算法的模版待整理

