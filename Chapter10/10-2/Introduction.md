# 一、概念

（1）数组的线性序是由数组的下标决定的，链表中的顺序是由各对象中的指针所决定的  

（2）链表结点结构  
node *prev;  
node *next;  
int key;  

（3）链表结点    
node *head;  
node *nil;//哨兵  

（4）对链表的操作  
LIST-SEARCH(L, k)  
LIST-INSERT(L, x)  
LIST-DELETE(L, x)  

（5）哨兵是个哑对象，可以简化边界条件  

# 二、代码

（1）没有哨兵的情况  

```c++
//链表结点结构
struct node
{
	node *pre;
	node *next;
	int key;
	//构造函数
	node(int x):pre(NULL),next(NULL),key(x){}
};
//链表结构
struct List
{
	node *head;
	List():head(NULL){}
};
//打印
void List_Print(List *L)
{
	node *p = L->head;
	while(p)
	{
		cout<<p->key<<' ';
		p = p->next;
	}
	cout<<endl;
}
//搜索，找出L中第一个关键字为k的结点，没有则返回NULL
node *List_Search(List *L, int k)
{
	node *x = L->head;
	while(x != NULL && x->key!=k)
		x = x->next;
	return x;
}
//插入
void List_Insert(List *L, node *x)
{
	//插入到表的前端
	x->next = L->head;
	if(L->head != NULL)
		L->head->pre = x;
	L->head = x;
	x->pre = NULL;
}
//删除
void List_Delete(List *L, node* x)
{
	if(x->pre != NULL)
		x->pre->next = x->next;
	else
		L->head = x->next;
	if(x->next != NULL)
		x->next->pre = x->pre;
	delete x;
}
```

（2）有哨兵的情况  

```c++
//链表结点结构
struct node
{
	node *pre;
	node *next;
	int key;
	//构造函数
	node(int x):pre(NULL),next(NULL),key(x){}
};
//链表结构
struct List
{
	node *nil;//哨兵
	List()
	{
		nil = new node(0);
		nil->next = nil;
		nil->pre = nil;
	}
};
//打印
void List_Print(List *L)
{
	node *p = L->nil->next;
	while(p != L->nil)
	{
		cout<<p->key<<' ';
		p = p->next;
	}
	cout<<endl;
}
//搜索，找出L中第一个关键字为k的结点，没有则返回NULL
node *List_Search(List *L, int k)
{
	node *x = L->nil->next;
	while(x != L->nil && x->key!=k)
		x = x->next;
	return x;
}
//插入
void List_Insert(List *L, node *x)
{
	//插入到表的前端
	x->next = L->nil->next;
	L->nil->next->pre = x;
	L->nil->next = x;
	x->pre = L->nil;
}
//删除
void List_Delete(List *L, node* x)
{
	x->pre->next = x->next;
	x->next->pre = x->pre;
	delete x;
}
```
