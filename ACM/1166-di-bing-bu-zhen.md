# 题目链接

http://acm.hdu.edu.cn/showproblem.php?pid=1166  

关键字：树状数组

# 题目分析

将一组数组s[N]

输入Query a b，输出SUM（sa + …… + sb）

输入Add a b，s[a] = s[a] + b

输入Sub a b，s[a] = s[a] - b

数组动态求和，明显的树状数组，调用树状数组模版：树状数组


# 总结：

原来用C++标准输入输出能过，现在TLE，要改用C的输入输出才行。

复习了一下树状数组

