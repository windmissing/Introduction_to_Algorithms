# 一、概念

1.对象的多重数组表示：对一组具有相同域的对象，每一个对象都可以用一个数组来表示

![](http://windmissing.github.io/images_for_gitbook/Introduction_to_Algorithms/4.gif)  

2.对象的单数组表示：一个对象占据存储中的一组连续位置

![](http://windmissing.github.io/images_for_gitbook/Introduction_to_Algorithms/5.gif)  

# 二、代码

```c++
int Allocate_Object()
{
	if(free == NULL)
	{
		cout<<"error:out of space"<<endl;
		exit(0);
	}
	else
	{
		int x = free;
		free = next[x];
		return x;
	}
}
void Free_Object(int x)
{
	next[x] = free;
	free = x;
}
```
