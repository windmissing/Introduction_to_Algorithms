```c++

/*
箱子和人共同组成一个状态，用node来记录状态
使用优先队列，是因为只有箱子移动，记数才+1，并不是每次都+1，
从队列中选择记数最小的，进行下一步搜索
只使用优先队列不能保证结果是最小。
因为两个记数相同的状态，下一步的记数不一定相同
使用flag避免重复计算
*/
#include <iostream>
#include <queue>
using namespace std;
 
struct node
{
	int ceil;
	int people_x,people_y;
	int box_x,box_y;
	bool operator<(const node& A)const
	{
		return ceil > A.ceil;
	}
};
int map[9][9];
priority_queue<node> Q;
int dir[4][2] = {{1, 0},{-1, 0},{0, 1},{0, -1}};
bool flag[9][9][9][9];
 
int main()
{
	int t, m, n, i, j, px,py,bx,by, ans;
	node p, q;
	cin>>t;
	while(t--)
	{
		cin>>m>>n;
		memset(map, 0, sizeof(map));
		memset(flag, 0, sizeof(flag));
		ans = -1;
		for(i = 0; i < m; i++)
		{
			for(j = 0; j < n; j++)
			{
				cin>>map[i][j];
				if(map[i][j] == 4)
				{
					p.people_x = i;
					p.people_y = j;
				}
				else if(map[i][j] == 2)
				{
					p.box_x = i; 
					p.box_y = j;
				}
			}
		}
		while(!Q.empty())Q.pop();
		p.ceil = 0;Q.push(p);
		flag[p.people_x][p.people_y][p.box_x][p.box_y] = 1;
		
		ans = -1;
		while(!Q.empty())
		{
			p = Q.top();Q.pop();
			px = p.people_x; py = p.people_y;
			bx = p.box_x; by = p.box_y;
			if(map[bx][by] == 3)
			{
				if(ans == -1 || ans > p.ceil)
					ans = p.ceil;
			}
			for(i = 0; i < 4; i++)
			{
				//越界
				if((px+dir[i][0])<0 || (px+dir[i][0]) >= m || (py+dir[i][1]) < 0 || (py+dir[i][1]) >= n)
					continue;
				//墙
				else if(map[px+dir[i][0]][py+dir[i][1]] == 1)
					continue;
				//不是箱子
				else if(px+dir[i][0] != bx || py+dir[i][1] != by)
				{
					q = p;
					q.people_x += dir[i][0];
					q.people_y += dir[i][1];
					if(flag[q.people_x][q.people_y][q.box_x][q.box_y] == 0)
					{
						flag[q.people_x][q.people_y][q.box_x][q.box_y] = 1;
						Q.push(q);
					}
				}
				//是箱子
				else if(px+dir[i][0] == bx && py+dir[i][1] == by)
				{
					//越界
					if((px+2*dir[i][0])<0 || (px+2*dir[i][0]) >= m || (py+2*dir[i][1]) < 0 || (py+2*dir[i][1]) >= n)
						continue;
					//墙
					else if(map[px+2*dir[i][0]][py+2*dir[i][1]] == 1)
						continue;
					//推箱子
					q.box_x = px+2*dir[i][0]; q.box_y = py+2*dir[i][1];
					q.people_x = px+dir[i][0];q.people_y = py+dir[i][1];
					q.ceil = p.ceil + 1;
					if(flag[q.people_x][q.people_y][q.box_x][q.box_y] == 0)
					{
						flag[q.people_x][q.people_y][q.box_x][q.box_y] = 1;
						Q.push(q);
					}
				}
			}
		}
		cout<<ans<<endl;
	}
	return 0;
}
```