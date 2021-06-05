/*HDU2451思想不是太难，要细心*/
#include <iostream>
#include <string>
#include <cmath>
using namespace std;
int main()
{
	string s;
	while(cin>>s)
	{
		int i, ans = 0,temp;
		for(i = 0; i < s.length(); i++)
		{	
			//获取第i位上数字的值
			temp = s[i] - '0';
			//如果是最后一位，可选择0、1、2
			if( i == s.length() - 1)
			{
				if(temp < 3)
					ans = ans + temp ;
				else ans = ans + 3;
				continue;
			}
			//如果是最后一位，可选择0、1、2、3
			if(temp < 4)
				ans = ans + temp * pow(4.0, (s.length()-2-i)*1.0) * 3;
			else
			{
				ans = ans + pow(4.0, (s.length()-i-1)*1.0) * 3;
				break;
			}
		}
		cout<<ans<<endl;
	}
	return 0;
}
