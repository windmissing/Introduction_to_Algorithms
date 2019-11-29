10.1-1

4 1

10.1-2

分别用数组的两端作为两个栈的起点，向中间扩展，两个栈中的元素总和不超过n时，两个栈不会相遇  
见10.1-2 用一个数组实现两个栈

10.1-3

3 8

10.1-4

```c++
//代码：能处理上溢和下溢的队列
//入队列
void Enqueue2(queue &Q, int x)
{
	int t;
	//上溢
	if(Q.tail == Q.length)
		t = 1;
	else t= Q.tail+1;
	if(t == Q.head)
	{
		cout<<"error:overflow"<<endl;
		return;
	}
	else
	{
		Q.s[Q.tail] = x;
		Q.tail = t;
	}
}
//出队列
int Dequeue2(queue &Q)
{
	//下溢
	if(Q.head == Q.tail)
	{
		cout<<"error:underflow"<<endl;
		return -1;
	}
	int x = Q.s[Q.head];
	if(Q.head == Q.length)
		Q.head = 1;
	else Q.head++;
	return x;
}
```

10.1-6

入队列：  
如果B栈不为空，依次弹出B栈中的值并压入A栈。然后把要入队列的值压入A栈O(n)  
出队列 ：  
如果A栈不为空，依次弹出A栈中的值并压入B栈，然后将B栈中栈顶位置的值出栈并返回这个值O(n)  

```c++
//用两个栈来模拟一个队列
//输出队列，栈1是从栈底到栈顶，栈2是从栈顶到栈底
void Print(stack S1, stack S2)
{
	int i;
	for(i = 1; i <= S1.top; i++)
		cout<<S1.s[i]<<' ';
	for(i = S2.top; i >= 1; i--)
		cout<<S2.s[i]<<' ';
	cout<<endl;
}
//判断队列是否为空
bool Queue_Empty(stack &S1, stack &S2)
{
	if(Stack_Empty(S1)&&Stack_Empty(S2))
		return 1;
	return 0;
}
//入队列
void Enqueue(stack &S1, stack &S2, int x)
{
	//如果B栈不为空，依次弹出B栈中的值并压入A栈
	while(!Stack_Empty(S2))
		Push(S1, Pop(S2));
	//要入队列的值压入A栈
	Push(S1, x);
}
//出队列
int Dequeue(stack &S1, stack &S2)
{
	//如果A栈不为空，依次弹出A栈中的值并压入B栈
	while(!Stack_Empty(S1))
		Push(S2, Pop(S1));
	//将B栈中栈顶位置的值出栈并返回这个值
	return Pop(S2);
}
```
 
10.1-7

入栈：  
两个队列中，其中一个是空队列，将值入这个空队列，然后将另一个非空队列的值依次取出并加入这个队列中O(n)  
出栈：直接从非空的队列中取出O(1)  

```c++
//用两个队列模拟栈
//判断栈是否为空
bool Stack3_Empty(queue Q1, queue Q2)
{
	if(Queue_Empty(Q1) && Queue_Empty(Q2))
		return 1;
	return 0;
}
//输出栈
void Print(queue Q1, queue Q2)
{
	if(!Queue_Empty(Q1))
		Print(Q1);
	if(!Queue_Empty(Q2))
		Print(Q2);
}
//入栈
void Push(queue &Q1, queue &Q2, int x)
{
	//两个队列中，其中一个是空队列
	if(Queue_Empty(Q1))
	{
		//将值入这个空队列
		Enqueue(Q1, x);
		//将另一个非空队列的值依次取出并加入这个队列中
		while(!Queue_Empty(Q2))
			Enqueue(Q1, Dequeue(Q2));
	}
	else//同理
	{
		Enqueue(Q2, x);
		while(!Queue_Empty(Q1))
			Enqueue(Q2, Dequeue(Q1));
	}
}
//出栈
int Pop(queue &Q1, queue &Q2)
{
	//直接从非空的队列中取出
	if(!Queue_Empty(Q1))
		return Dequeue(Q1);
	if(!Queue_Empty(Q2))
		return Dequeue(Q2);
	cout<<"error:underflow"<<endl;
	return -1;
}
```