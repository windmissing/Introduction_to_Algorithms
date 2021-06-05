#### 题目链接

http://acm.hdu.edu.cn/showproblem.php?pid=1394

Inversion Number, 树状数组

#### 题目解释

对于一个由n个数字组成的序列，把它的队首m(0<=m<n)个元素移到队尾，求形成了另一个队列

长度为n的序列通过这种方式可以得到n种不同的序列，求这n个序列中逆序数最小的序列的逆序数

<!-- more -->

#### 算法

##### 关于树状数组

参考《树状数组》

##### 由树状数组求原始数组的逆序对

对原始序列中的所有元素，其元素值+1（因为树状数组不能处理0，所有必须保证要处理的序列中没有值为0的元素）

对原始序列中的元素，从队尾至队首，依次对每个元素做以下操作：

1.把该元素q加入到树状数组a中，即a.modify(q, 1)

2.计算在q之前加入到a且比q小的元素个数，即ret = getsum(i - 1)

3.该序列的逆序对数 += ret

当序列中的元素全部处理完，就能计算出该序列的逆序对数了

##### 依次求各种序列的逆序对数

令f(m)为把原始序列s的前m个元素移到队尾得到的序列的逆序对数

现在已求出f(0)

第一个元素q移到队尾后，f(1)=f(0)+比q大的元素个数-比q小的元素个数

依次类推，f(m) = f(m-1) + 比s[m-1]大的元素个数 - 比s[m-1]小的元素个数

这样就可以依次计算出所有序列的逆序对数了

#### 代码

```c++
    #include <stdio.h>  
    #define MAXN 10000  
      
    int c[MAXN],a[MAXN],n;  
      
    int min(int a,int b)  
    {  
        if (a < b) return a;  
        else return b;  
    }  
      
    int lowbit(int i)  
    {  
        return i & -i;  
    }  
      
      
    void update(int i,int x)  
    {  
        while (i <= n)  
        {  
            c[i] += x;  
            i += lowbit(i);  
        }  
    }  
      
    int getsum(int x)  
    {  
        int sum = 0;  
        while (x > 0)  
        {  
            sum += c[x];  
            x -= lowbit(x);  
        }  
        return sum;  
    }  
      
    int main()  
    {  
        while (scanf("%d", &n) == 1)  
        {  
            for (int i = 1; i <= n; i++)  
                c[i] = 0;  
            int sum = 0;  
            for (int i = 1; i <= n; i ++)  
            {  
                scanf ("%d", &a[i]);  
            }  
            for (int i = n; i >= 1; i --)  
            {  
                update (a[i] + 1, 1);  
                sum += getsum (a[i]);  
            }  
            int ans = sum;  
            for (int i = 1; i <= n; i++)  
            {  
                sum = sum - a[i] + (n - a[i] - 1);  
                ans = min(ans, sum);  
            }  
            printf("%d\n", ans);  
        }  
        return 0;  
    }  
```
