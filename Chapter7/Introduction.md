# 一、概念

快速排序是基于分治模式的，选择一个数作为主元，经过一遍扫描，所有小于主元的数放在主元的左边，大于主元的数放在主元的右边，这样就划分成了两组数据。然后对两组数分别进行快排。

快排的运行时间与划分是否对称有关，关键是如何选择主元。

最坏情况下，时间复杂度是O(n^2)，最好情况下，时间是O(nlgn)

# 二、程序

[头文件](https://github.com/windmissing/exerciseForAlgorithmSecond/blob/master/include/chapter7/quickSort.h)

[算法过程](https://github.com/windmissing/exerciseForAlgorithmSecond/blob/master/src/chapter7/quickSort.cpp)

[测试](https://github.com/windmissing/exerciseForAlgorithmSecond/blob/master/tst/chapter7/quickSortTest.cpp)
