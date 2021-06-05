```c++

//1181floyd传递包
#include <iostream>
using namespace std;
/***********************************************/
bool graph[26][26];
void floyd()
{
	int i,j,k;
	for(k=0;k<26;k++)
	{
		for(i=0;i<26;i++)
		{
			for(j=0;j<26;j++)
			{
				if(graph[i][k]&&graph[k][j])
					graph[i][j]=1;
 
			}
		}
	}
}
/************************************************/
int main()
{
	memset(graph,0,sizeof(graph));
    char ch[80];
	int a,b;
    while(cin>>ch)
    {
		if(ch[0]=='0')
		{
			floyd();
			if(graph[1][12])
				cout<<"Yes."<<endl;
			else cout<<"No."<<endl;
			memset(graph,0,sizeof(graph));
		}
		else
		{
			a=ch[0]-'a';
			int j=strlen(ch);
			b=ch[j-1]-'a';
			graph[a][b]=1;
		}
 
	}
    return 0;
}
```