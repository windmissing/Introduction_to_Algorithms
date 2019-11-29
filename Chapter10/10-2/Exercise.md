10.2-1

能，能

10.2-2

```c++
//结点
struct node
{
	node *pre;//为了方便实现出栈操作
	node *next;
	int key;
	node(int x):pre(NULL),next(NULL),key(x){}
};
//链式栈
struct list
{
	node *Head;//栈的起始结点
	node *Top;//栈顶指针
	list(){Head = new node(0);Top = Head;}
};
//打印栈中的元素
void Print(list *L)
{
	node *p = L->Head->next;
	while(p)
	{
		cout<<p->key<<' ';
		p = p->next;
	}
	cout<<endl;
}
//入栈
void Push(list *L, int x)
{
	//构造一个新的结点
	node *A = new node(x);
	//链入到栈顶位置，修改指针
	L->Top->next = A;
	A->pre = L->Top;
	L->Top = A;
}
//出栈
int Pop(list *L)
{
	if(L->Head == L->Top)
	{
		cout<<"error:underflow"<<endl;
		return -1;
	}
	//取出栈顶元素
	int ret = L->Top->key;
	//修改指针
	node *A = L->Top;
	L->Top = A->pre;
	L->Top->next = NULL;
	delete A;
	return ret;
}
```

10.2-3

```c++
//结点
struct node
{
	node *next;
	int key;
	node(int x):next(NULL),key(x){}
};
//链式队列
struct list
{
	node *Head;//头指针
	node *Tail;//尾指针
	list(){Head = new node(0);Tail = Head;}
};
//打印
void Print(list *L)
{
	node *p = L->Head->next;
	while(p)
	{
		cout<<p->key<<' ';
		p = p->next;
	}
	cout<<endl;
}
//入队列
void Enqueue(list *L, int x)
{
	//构造一个新的结点
	node *A = new node(x);
	//链入尾部，修改指针
	L->Tail->next = A;
	L->Tail = A;
}
//出队列
int Dequeue(list *L)
{
	if(L->Head == L->Tail)
	{
		cout<<"error:underflow"<<endl;
		return -1;
	}
	//取出队头结点，修改指针
	node *A = L->Head->next;
	int ret = A->key;
	L->Head->next = A->next;
	if(A == L->Tail)
		L->Tail = L->Head;
	delete A;
	return ret;
}
```

10.2-4

把哨兵的值置为一个不可能与x相等的值

10.2-5

见算法导论 10.2-5 环形链表实现字典操作INSERT、DELETE、SEARCH

10.2-6

使用带尾指针的链表，令A的尾指针为tail，tail->next=B

10.2-7

```c++
//逆转
void Reverse(list *L)
{
	node *p = NULL, *q = L->Head, *r;
	//依次修改指针，让q是p->next,令q->next=p
	while(1)
	{
		r = q->next;
		q->next = p;
		if(r == NULL)
		{
			L->Head = q;
			break;
		}
		p = q;
		q = r;
	}
}
```
10.2-8

见算法导论-10.2-8