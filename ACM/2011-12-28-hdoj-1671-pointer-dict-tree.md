
用指针传递字典树，时空效率更高  
用字符串代替字符数组，更方便  

```c++
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;
string str[10005];
struct dictree
{
   struct dictree *child[10];
   int n;
   dictree(){ memset(child,0,sizeof(child));n=0;}
};
bool insert(dictree* root,string s)
{
   int len,i,j;
   struct dictree *current,*newnode;
   len=s.length();
   if(len==0)return 0;
   current=root;
   for(i=0;i<len;i++)
   {
       if(current->n == 1)
           return 1;
       if(current->child[s[i]-'0']!=0)
           current=current->child[s[i]-'0'];
       else
       {
           newnode=(struct dictree *)malloc(sizeof(struct dictree));
           for(j=0;j<10;j++)
               newnode->child[j]=0;
           current->child[s[i]-'0']=newnode;
           current=newnode;
           current->n=0;
       }
       if(i == len - 1)current->n=1;
   }
   return 0;
}
bool cmp(string s1, string s2)
{
   return s1.length() < s2.length();
}
void clear(dictree* root)
{
   if(root == NULL)
       return;
   for(int i = 0; i < 10; i++)
       clear(root->child[i]);
   delete root;
}
int main()
{
   int t, n, i;
   cin>>t;
   while(t--)
   {
       dictree *root = new dictree;
       bool flag = 0;
       cin>>n;
       for(i = 0; i < n; i++)
           cin>>str[i];
       sort(str, str+n, cmp);
       for(i = 0; i < n; i++)
       {
           if(insert(root, str[i]))
           {
               flag = 1;
               break;
           }
       }
       if(flag == 1)
           cout<<"NO"<<endl;
       else
           cout<<"YES"<<endl;
       clear(root);
   }
   return 0;
}
```