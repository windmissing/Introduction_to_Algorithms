```c++
//1160
#include <iostream>
#include <algorithm>
using namespace std;
 
struct mouse
{
	int id;
	int weight;
	int speed;
	int pos;
	int pre;
}s[10005];
bool cmp(mouse a,mouse b)
{
	return(a.weight<b.weight||(a.weight==b.weight&&a.speed<b.speed));
}
 
int main()
{
	int count=0;
	while(scanf("%d %d",&s[count].weight,&s[count].speed)!=EOF)
	{
		s[count].id=count+1;
		count++;
	}
	sort(s,s+count,cmp);
/*********************************************************************************/
//最长递增子序列部分
	int ans[10005]={0};
	s[0].pos=1;
	int i,j,max=1,maxi=0;
	//记录每个点的序列位置
	for(i=1;i<count;i++)
	{
		j=max;
		while(j)
		{
			if(s[i].weight>s[ans[j]].weight&&s[i].speed<s[ans[j]].speed)
			{
				s[i].pos=j+1;
				ans[j+1]=i;
				s[i].pre=ans[j];
				break;
			}
			j--;
		}
		//第一个点
		if(j==0)
		{
			s[i].pos=1;
			ans[1]=i;
		}
		//目前为止最长的点
		if(s[i].pos>max)
		{
			max=s[i].pos;
			maxi=i;
		}
	}
/*	
	for(int cc=0;cc<count;cc++)
		cout<<cc<<' '<<s[cc].id<<' '<<s[cc].weight<<' '<<s[cc].speed<<' '<<s[cc].pos<<' '<<s[cc].pre<<endl;
*/
 
	cout<<max<<endl;
	//输出最长序列
	int temp[10005];
	for(i=1;i<=max;i++)
	{
		temp[i]=s[maxi].id;
		maxi=s[maxi].pre;
	}
	for(i=max;i>0;i--)
		cout<<temp[i]<<endl;
/*********************************************************************************/
 
	return 0;
}
```