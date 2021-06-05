# 题目链接：

http://acm.hdu.edu.cn/showproblem.php?pid=1053

关键字：哈夫曼编码
 
# 题目解释：

输入一个字符串（只包含26个大写字母和‘_'），每个字母8位，这个字符串为多少位？若采用哈夫曼编码，字符串多少位，压缩率为多少（1位小数）？
 
# 常规方法：

用优先队列使用哈夫曼树，计算每个字符的哈夫曼编码，那么字符串的总位数=SUM（每个字符编码的长度*字符出现的次数）。求哈夫曼编码的过程如下：

step1：统计每个字符出现的次数，每个字符作为一个结点，以出现的次数为结点的权值，存入优先队列中

step2：取队列中的前两个结点，合并成一个结点，更新结点的权值，插入优先队列中

step3：当队列中只剩下一个元素时，哈夫曼编码过程完成
 
# 优化方法：

事实上，本题只求字符串的总位数，没有要求求出每个字符的编码，哈夫曼编码过程与求字符串位数的过程可以同时进行。

令L(T)为T树对应的字符串的总位数，t为T的根结点的权值。

对于一棵用每个字符出现的次数构造出来的哈夫曼树T，L(T)=SUM（每个字符编码的长度*字符出现的次数）=SUM（每个叶子的高度*叶子的权值）。

在Step1中，每个字符作为一个结点，或者说是只有一个结点的哈夫曼树。当树中只有一个结点时，叶子的高度为1，L(T)=t。

在Step2中，每次取其中两棵哈夫曼树T1，T2进行合并，成为一棵新的哈夫曼树T。T1、T2分别成为T的左右孩子。T1和T2成为子树后，每个叶子的高度都+1，新L(T1)=原L(T1)+T1中每个叶子的权值和，根据哈夫曼树的定义，T1中每个叶子的权值和=t1；T2类似。L(T)=新L(T1)+新L(T2)=原L(T1)+t1+原L(T2)+t2 = 原L(T1)+原L(T2)+t

为了简化编程，优先队列中只需要存储每个T的权值t
 
# 代码：

```c++
#include <iostream>   
#include <iomanip>
#include <queue>   
#include <string>   
using namespace std;  ;  

int main(){ 
	string s;
	while(cin>>s){  
		if(s == "END")break;  

		int pl[27],len;
		//统计频度    
		memset(pl,0,sizeof pl);  
		len=s.length();  
		for(int i=0;i<len;i++){  
			if(s[i]=='_')pl[0]++;  
			else pl[s[i]-'A'+1]++;  
		}  
		//判断是否单一字符   
		bool yes=0;   
		for(int i=0;i<27;i++){  
			if(pl[i]==len){  
				cout<<len*8<<' '<<len<<' '<<8.0<<endl;
				yes=1;  
				break;    
			}     
		}  
		if(yes)continue;  

		//使用优先队列统计HUFFMAN编码    
		priority_queue<int,vector<int>,greater<int> > q;   
		for(int i=0;i<27;i++)
			if(pl[i]!=0)
				q.push(pl[i]);  
		int ans=0,a,b; 
		while(1){  
			a=q.top();q.pop();  
			if(q.empty())break;  
			b=q.top();q.pop();  
			ans+=a+b;  
			q.push(a+b);  
		}   
		cout<<len*8<<' '<<ans<<' ';
		cout<<setiosflags(ios::fixed)<<setprecision(1)<<(len*8.0/ans)<<endl;
	}     
	return 0;     
}
```

# 总结：

C++中一位小数的输出方式：

```c++
#include <iomanip>
cout<<setiosflags(ios::fixed)<<setprecision(1)<<ans<<endl;
```

STL中优先队列的使用方式：

```c++
#include <queue>
priority_queue<int, vector<int>, greater<int>> q;
``` 
 


 
 
