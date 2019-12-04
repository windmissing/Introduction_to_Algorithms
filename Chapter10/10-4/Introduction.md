# 一、概念

## 1.二叉树

（1）用域p、left、right来存放指向二叉树T中的父亲、左儿子、右儿子。没有则为NULL。

（2）结点结构

```c++
struct node
{
    node *p;
    node *left;
    node *right;
    int key;
};
```

（3）树的结构

```c++
struct Bin_Tree
{
    node *head;
};
```

## 2.分支数无限的有根树

（1）左孩子右兄弟表示法

（2）结点结构

```c++
struct node
{
    node *p;
    node *left_child;
    node *right_sibling;
    int key;
};
```

（3）树的结构

```c++
struct Tree
{
    node *head;
};
```

