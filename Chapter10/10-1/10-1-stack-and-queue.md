# 一、概念

## 1.栈

（1）栈实现了后进先出操作。  
在栈的数组实现中，栈顶指针指向栈顶元素，插入时先修改指针再插入，删除时先取栈顶元素再修改指针。  
当top[S]=0时，栈中空的。  

（2）数组栈的结构：  
int top;//栈顶指针  
int *s];//指向栈数组  

（3）在栈上实现的操作  
STACK-EMPTY(S)//判断栈是否为空  
PUSH(S, x)            //把x压入到栈顶  
POP(S)                 //取出并返回栈顶元素  

## 2.队列

（1）队列实现了先进先出操作。  
在队列的数组实现中，队列的头指针指向队列首元素，删除时先取队列首元素再修改指针，队列的尾指针指向队尾元素的下一个元素，插入时先插入再修改指针  
当top[S]=0时，栈中空的。  

（2）数组队列的结构：  
int tail;//队列尾，指向最新进入的元素  
int head;//队列头，指向最先出的元素  
int length;//队列的长度   
int *s;//指向数组队列  

（3）在队列上实现的操作  
ENQUEUE(Q, x)            //把x插入到队列尾  
DEQUEUE(Q)                //取出队列首元素并返回  

# 二、代码

```c++
//10.1栈和队列
#include <iostream>
#include <stdio.h>
#include <string>
using namespace std;

/*****************栈*******************************************************/
struct stack
{
	int top;//栈顶指针
	int *s;//数组
	stack(int size):top(0){s = new int[size+1];}
//	~stack(){delete []s;}
};
void Print(stack S)
{
	int i;
	//从栈底到栈顶的顺序输出
	for(i = 1; i <= S.top; i++)
		cout<<S.s[i]<<' ';
	cout<<endl;
}
//检查一个栈是否为空
bool Stack_Empty(stack &S)
{
	if(S.top == 0)
		return true;
	else
		return false;
}
//入栈
void Push(stack &S, int x)
{
	S.top++;
	S.s[S.top] = x;
}
//出栈
int Pop(stack &S)
{
	if(Stack_Empty(S))
	{
		cout<<"underflow"<<endl;
		exit(0);
	}
	else
	{
		S.top--;
		return S.s[S.top+1];
	}
}
/*****************队列********************************************************/
struct queue
{
	int tail;//队列头指针
	int head;//队列尾指针
	int length;//队列长度
	int *s;//数组
	queue(int size):tail(1),head(1),length(size){s = new int[size+1];}
};
//从队列头到队列尾输出
void Print(queue Q)
{
	int i;
	if(Q.tail >= Q.head)
	{
		for(i = Q.head; i < Q.tail;i++)
			cout<<Q.s[i]<<' ';
		cout<<endl;
	}
	//因为循环的原因，队列尾可能在队列头的前面
	else
	{
		for(i = Q.head; i <= Q.length; i++)
			cout<<Q.s[i]<<' ';
		for(i = 1; i < Q.tail; i++)
			cout<<Q.s[i]<<' ';
		cout<<endl;
	}
}
//判断队列是否为空
bool Queue_Empty(queue Q)
{
	if(Q.tail == Q.head)
		return 1;
	return 0;
}
//入队列
void Enqueue(queue &Q, int x)
{
	Q.s[Q.tail] = x;
	if(Q.tail == Q.length)
		Q.tail = 1;
	else Q.tail++;
}
//出队列
int Dequeue(queue &Q)
{
	int x = Q.s[Q.head];
	if(Q.head == Q.length)
		Q.head = 1;
	else Q.head++;
	return x;
}
```
