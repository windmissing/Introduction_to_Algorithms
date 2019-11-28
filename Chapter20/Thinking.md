##### 20-1删除的另一种实现
把FIB-HEAP-DELETE中的两个函数展开，再和PISANO-DELETE对比，并附上x不是min[H]的假设，可以发现这两个函数执行的操作基本上是一样的，区别在于  
（1）PISANO-DELETE中去掉了FIB-HEAP-DELETE中多余的判断，不影响效率  
（2）FIB-HEAP-DELETE在删掉结点之后有合并调整的动作  
a)add x's child list to the root list of H的时间不是O(1)，因为每个child都有一个pParent指针，必须依次修改每个child的指针  
20-2其它斐波那契堆的操作  
a)  
（1）k < key[x]的情况，直接调用FIB-HEAP-DECREASE-KEY  
（2）k = key[x]的情况，不用处理  
（3）k > key[x]的情况，交换它与它孩子的内容，但是指针保持不变，直到符合最小堆的情况，时间与堆的深度有关  
b)  
以我有限的智商，只能想到执行min(r, n[H])次EXTRACT  