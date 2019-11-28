---
layout: post
title:  "算法导论 12-2 基数树"
category: [算法导论]
tags: []
---

#### 题目

> 给定两个串a = a0a1……ap和b = b0b1……b1，其中每一个ai和每一个bj都属于某个有序字符集，如果下面两条规则之一成立，则说串a按字典序小于串b：  
1）存在一个整数j，0<=j<=min(p,q)，使得ai=bi，i=0，1，……，j-1，且aj<bj；  
2）p<q，且ai=bi，对所有的i=0，1，……，p成立  
例如，如果a和b是位串，则根据规则1）（设j=3），有10100 < 10110；根据规则2），有10100 < 101000。这与英语字典中的排序很相似。  
图12-5中示出的是基数树（radix tree）数据结构，其中存储了位串1011、10、011、100和0。当查找某个关键字a = a0a1……ap时，在深度为i的一个结点处，若ai = 0则向左转；若ai = 1则向右转。设S为一组不同的二进制串构成的集合，各串的长度之和为n。说明如何利用基数树，在O(n)时间内对S按字典序排序。例如，对图12-5中每个结点的关键字，排序的输出应该是序列0、011、10、100、1011  
#### 思考：

就是字典树的一种特殊情况，字典树的模版见用指针传递的字典树
这题在以前的题目里面出现过，使用了另一种解法，见[算法导论-8-3-排序不同长度的数据项中的b](http://blog.csdn.net/mishifangxiangdefeng/article/details/7686099)  

#### 代码：

```c++
#include <iostream>  
#include <string>  
using namespace std;  
  
//基数树结点结构  
struct node  
{  
    node *left;  
    node *right;  
    node *p;  
    string str;  
    node():left(NULL),right(NULL),p(NULL),str(""){}  
};  
//基数树结点  
struct Radix_Tree  
{  
    node *root;  
    Radix_Tree(){root = new node;}  
};  
//向基数树中插入一个位串  
void Insert(Radix_Tree *T, string s)  
{  
    int i;  
    node *t = T->root, *p;  
    //顺着串依次向下找  
    for(i = 0; i <s.length(); i++ )  
    {  
        //如果第i位是0，把串插入到左子树的某个位置  
        if(s[i] == '0')  
        {  
            //还没有左子树，就开辟一个  
            if(t->left == NULL)  
            {  
                p = new node;  
                p->p = t;  
                t->left = p;  
            }  
            else  
                p = t->left;  
        }  
        //如果第i位是1，把串插入到左子树的某个位置  
        else  
        {  
            //还没有左子树，就开辟一个  
            if(t->right == NULL)  
            {  
                p = new node;  
                p->p = t;  
                t->right = p;  
            }  
            else  
                p = t->right;  
        }  
        //一直找到串的最后一个字符，把串的内容存入这个节点中  
        if(i == s.length() - 1)  
            p->str = s;  
        else t = p;  
    }  
}  
//按字典序输出，其实是先序遍历的过程  
void Print(node *t)  
{  
    if(t == NULL)  
        return;  
    //如果某个节点有串的信息，就将它输出  
    if(t->str != "")  
        cout<<t->str<<endl;  
    Print(t->left);  
    Print(t->right);  
}  
int main()  
{  
    string str, x;  
    Radix_Tree *T = new Radix_Tree;  
    while(1)  
    {  
        cin>>str;  
        if(str == "I")  
        {  
            cin>>x;  
            Insert(T, x);  
        }  
        else if(str == "P")  
        {  
            Print(T->root);  
            cout<<endl;  
        }  
    }  
    return 0;  
}  
```
