---
layout: post 
title:  "后缀数组"
categories: 编程语言
tags: []
---

# 概述　　
在字符串处理当中，后缀树和后缀数组都是非常有力的工具，其中后缀树大家了解得比较多，关于后缀数组则很少见于国内的资料。其实后缀数组是后缀树的一个非常精巧的替代品，它比后缀树容易编程实现，能够实现后缀树的很多功能而时间复杂度也不太逊色，并且，它比后缀树所占用的空间小很多。
# 基本定义
## 1.子串
字符串 S 的子串 r[i..j] ， i ≤ j ，表示 r 串中从 i 到 j 这一段，就是顺次排列 r[i],r[i+1],...,r[j] 形成的字符串。
## 2.后缀
后缀是指从某个位置 i 开始到整个串末尾结束的一个特殊子串。字符串 r 的从 第 i 个字 符 开 始 的 后 缀 表 示 为 Suffix(i) ，也 就 是Suffix(i)=r[i..len(r)] 。
## 3.大小比较
关于字符串的大小比较，是指通常所说的 “ 字典顺序 ” 比较
## 4.后缀数组
后缀数组SA是一个一维数组，它保存1..n的某个排列SA[1]，SA[2]，……，SA[n]，并且保证Suffix(SA[i])<Suffix(SA[i+1])，1≤i<n。也就是将S的n个后缀从小到大进行排序之后把排好序的后缀的开头位置顺次放入SA中。
## 5.名次数组
名次数组 Rank[i] 保存的是 Suffix(i) 在所有后缀中从小到大 排列的 “ 名次 ” 。 　　 容易看出，后缀数组和名次数组为互逆运算。设字符串的长度为 n 。为了方便比较大小，可以在字符串后面添加一个字符，这个字符没有在前面的字符中出现过，而且比前面的字符都要小。在求出名次数组后，可以仅用 O(1) 的时间比较任意两个后缀的大小。在求出后缀数组或名次数组中的其中一个以后，便可以用 O(n) 的时间求出另外一个。任意两个后缀如果直接比较大小，最多需要比较字符 n 次，也就是说最迟在比较第 n 个字符时一定能分出 “ 胜负 ” 。
# 倍增算法
## 1.主要思路
倍增算法(Doubling Algorithm)，它正是充分利用了各个后缀之间的联系，将构造后缀数组的最坏时间复杂度成功降至O(nlogn)。
对一个字符串u，我们定义u的k-前缀，以及k-前缀比较关系<k、=k和≤k：
设两个字符串u和v，
　　u<kv 当且仅当 uk<vk
　　u=kv 当且仅当 uk=vk
　　u≤kv 当且仅当 uk≤vk
直观地看，这些加了一个下标k的比较符号的意义就是对两个字符串的前k个字符进行字典序比较。根据前缀比较符的性质我们可以得到以下的非常重要的性质：
　　性质1.1 对k≥n，Suffix(i)<kSuffix(j) 等价于 Suffix(i)<Suffix(j)。
性质1.2 Suffix(i)=2kSuffix(j)等价于 Suffix(i)=kSuffix(j)且Suffix(i+k)=kSuffix(j+k)。
　　 性质1.3 Suffix(i)<2kSuffix(j) 等价于 Suffix(i)<kS(j) 或 (Suffix(i)=kSuffix(j)且Suffix(i+k)<kSuffix(j+k))。
定义k-后缀数组SAk保存1..n的某个排列SAk[1],SAk[2],…SAk[n]使得Suffix(SAk) ≤kSuffix(SAk[i+1]),1≤i<n。也就是说对所有的后缀在k-前缀比较关系下从小到大排序，并且把排序后的后缀的开头位置顺次放入数组SAk中。
定义k-名次数组Rankk，Rankk代表Suffix(i)在k-前缀关系下从小到大的“名次”，也就是1加上满足Suffix(j)<kSuffix(i)的j的个数。通过SAk很容易在O(n)的时间内求出Rankk。
假设我们已经求出了SAk和Rankk，那么我们可以很方便地求出SA2k和Rank2k，因为根据性质1.2和1.3，2k-前缀比较关系可以由常数个k-前缀比较关系组合起来等价地表达，而Rankk数组实际上给出了在常数时间内进行<k和=k比较的方法，即： 　　Suffix(i)<kSuffix(j) 当且仅当 Rankk[i]<Rankk[j] 　　Suffix(i)=kSuffix(j) 当且仅当 Rankk[i]=Rankk[j]。因此，比较Suffix(i)和Suffix(j)在k-前缀比较关系下的大小可以在常数时间内完成，于是对所有的后缀在≤k关系下进行排序也就和一般的排序没有什么区别了，它实际上就相当于每个Suffix(i)有一个主关键字Rankk[i]和一个次关键字Rankk[i+k]。如果采用快速排序之类O(nlogn)的排序，那么从SAk和Rankk构造出SA2k的复杂度就是O(nlogn)。更聪明的方法是采用基数排序，复杂度为O(n)。
求出SA2k之后就可以在O(n)的时间内根据SA2k构造出Rank2k。因此，从SAk和Rankk推出SA2k和Rank2k可以在O(n)时间内完成。
下面只有一个问题需要解决：如何构造出SA1和Rank1。这个问题非常简单：因为<1，=1和≤1这些运算符实际上就是对字符串的第一个字符进行比较，所以只要把每个后缀按照它的第一个字符进行排序就可以求出SA1，不妨就采用快速排序，复杂度为O(nlogn)。 　　于是，可以在O(nlogn)的时间内求出SA1和Rank1。
求出了SA1和Rank1，我们可以在O(n)的时间内求出SA2和Rank2，同样，我们可以再用O(n)的时间求出SA4和Rank4，这样，我们依次求出：SA2和Rank2，SA4和Rank4，SA8和Rank8，……直到SAm和Rankm，其中m=2k且m≥n。
根据性质1.1，SAm和SA是等价的。这样一共需要进行logn次O(n)的过程，因此，可以在O(nlogn)的时间内计算出后缀数组SA和名次数组Rank。
## 2.算法分析
倍增算法的时间复杂度比较容易分析。每次基数排序的时间复杂度为O(n)，排序的次数决定于最长公共子串的长度，最坏情况下，排序次数为logn次，所以总的时间复杂度为O(nlogn)。
最长公共前缀
现在一个字符串S的后缀数组SA可以在O(nlogn)的时间内计算出来。利用SA我们已经可以做很多事情，比如在O(mlogn)的时间内进行模式匹配，其中m,n分别为模式串和待匹配串的长度。但是要想更充分地发挥后缀数组的威力，我们还需要计算一个辅助的工具——最长公共前缀（Longest Common Prefix）。
对两个字符串u,v定义函数lcp(u,v)=max{i|u=iv}，也就是从头开始顺次比较u和v的对应字符，对应字符持续相等的最大位置，称为这两个字符串的最长公共前缀。
对正整数i,j定义LCP(i,j)=lcp(Suffix(SA[i]),Suffix(SA[j]))，其中i,j均为1至n的整数。LCP(i,j)也就是后缀数组中第i个和第j个后缀的最长公共前缀的长度。
关于LCP有两个显而易见的性质：
　　性质2.1 LCP(i,j)=LCP(j,i)
　　性质2.2 LCP(i,i)=len(Suffix(SA[i]))=n-SA[i]+1
这两个性质的用处在于，我们计算LCP(i,j)时只需要考虑i<j的情况，因为i>j时可交换i,j，i=j时可以直接输出结果n-SA+1。
用顺次比较对应字符的方法来计算LCP(i,j)，时间复杂度为O(n)，进行适当的预处理可以降低每次计算LCP的复杂度。
经过仔细分析，我们发现LCP函数有一个非常好的性质：
设i<j，则LCP(i,j)=min{LCP(k-1,k)|i+1≤k≤j} （LCP Theorem）
要证明LCP Theorem，首先证明LCP Lemma:对任意1≤i<j<k≤n，LCP(i,k)=min{LCP(i,j),LCP(j,k)}
证明：设p=min{LCP(i,j),LCP(j,k)}，则有LCP(i,j)≥p,LCP(j,k)≥p。
设Suffix(SA)=u,Suffix(SA[j])=v,Suffix(SA[k])=w。
由u =LCP(i,j) v得u =p v；同理v =p w。
于是Suffix(SA[i]) =p Suffix(SA[k])，即LCP(i,k)≥p。 (1)
又设LCP(i,k)=q>p，则 　　u[1]=w[1],u[2]=w[2],...u[q]=w[q]。
而min{LCP(i,j),LCP(j,k)}=p说明u[p+1]≠v[p+1]或v[p+1]≠w[q+1]，
设u[p+1]=x,v[p+1]=y,w[p+1]=z，显然有x≤y≤z，又由p<q得p+1≤q，应该有x=z，也就是x=y=z，这与u[p+1]≠v[p+1]或v[p+1]≠w[q+1]矛盾。
于是，q>p不成立，即LCP(i,k)≤p。 (2)
综合(1),(2)知 LCP(i,k)=p=min{LCP(i,j),LCP(j,k)}，LCP Lemma得证。
于是LCP Theorem可以证明如下：
当j-i=1和j-i=2时，显然成立。
设j-i=m时LCP Theorem成立，当j-i=m+1时， 　　由LCP Lemma知LCP(i,j)=min{LCP(i,i+1),LCP(i+1,j)}， 　　因j-(i+1)≤m，LCP(i+1,j)=min{LCP(k-1,k)|i+2≤k≤j}，故当j-i=m+1时，仍有 　　LCP(i,j)=min{LCP(i,i+1),min{LCP(k-1,k)|i+2≤k≤j}}=min{LCP(k-1,k}|i+1≤k≤j) 　　根据数学归纳法，LCP Theorem成立。
根据LCP Theorem得出必然的一个推论：
LCP Corollary 对i≤j<k，LCP(j,k)≥LCP(i,k)。
定义一维数组height，令height[i]=LCP(i-1,i)，1<i≤n，并设height[1]=0。
由LCP Theorem，LCP(i,j)=min{height[k]|i+1≤k≤j}，也就是说，计算LCP(i,j)等同于询问一维数组height中下标在i+1到j范围内的所有元素的最小值。对于一个固定的字符串S，其height数组显然是固定的。求出height数组后，这个问题就转化为RMQ（Range Minimum Query）问题了。
于是只有一个问题——如何尽量高效地算出height数组。根据计算后缀数组的经验，我们不应该把n个后缀看作互不相关的普通字符串，而应该尽量利用它们之间的联系，下面证明一个非常有用的性质：
为了描述方便，设h[i]=height[Rank[i]]，即height[i]=h[SA[i]]。h数组满足一个性质：
性质3 对于i>1且Rank[i]>1，一定有h[i]≥h[i-1]-1。 　　
为了证明性质3，我们有必要明确两个事实： 　　
设i<n,j<n，Suffix(i)和Suffix(j)满足lcp(Suffix(i),Suffix(j)>=1，则成立以下两点：
　　Fact 1 Suffix(i)<Suffix(j) 等价于 Suffix(i+1)<Suffix(j+1)。
　　Fact 2 一定有lcp(Suffix(i+1),Suffix(j+1))=lcp(Suffix(i),Suffix(j))-1。 　　
看起来很神奇，但其实很自然：lcp(Suffix(i),Suffix(j))>=1说明Suffix(i)和Suffix(j)的第一个字符是相同的，设它为α，则Suffix(i)相当于α后连接Suffix(i+1)，Suffix(j)相当于α后连接Suffix(j+1)。比较Suffix(i)和Suffix(j)时，第一个字符α是一定相等的，于是后面就等价于比较Suffix(i)和Suffix(j)，因此Fact 1成立。Fact 2可类似证明。 　　
于是可以证明性质3：
　当h[i-1]≤1时，结论显然成立，因h≥0≥h[i-1]-1。
　当h[i-1]>1时，也即height[Rank[i-1]]>1，可见Rank[i-1]>1，因height[1]=0。 　　
令j=i-1,k=SA[Rank[j]-1]。显然有Suffix(k)<Suffix(j)。 　　
根据h[i-1]=lcp(Suffix(k),Suffix(j))>1和Suffix(k)<Suffix(j)： 　　
由Fact 2知lcp(Suffix(k+1),Suffix(i))=h[i-1]-1。 　　
由Fact 1知Rank[k+1]<Rank[i]，也就是Rank[k+1]≤Rank[i]-1。 　　
根据LCP Corollary，有
LCP(Rank[i]-1,Rank[i])≥LCP(Rank[k+1],Rank[i])=lcp(Suffix(k+1),Suffix(i)) =h[i-1]-1 　　
由于h[i]=height[Rank[i]]=LCP(Rank[i]-1,Rank[i])，最终得到 h[i]≥h[i-1]-1。 　　
根据性质3，可以令i从1循环到n按照如下方法依次算出h[i]：若Rank[i]=1，则h[i]=0。字符比较次数为0。若i=1或者h[i-1]≤1，则直接将Suffix(i)和Suffix(Rank-1)从第一个字符开始依次比较直到有字符不相同，由此计算出h[i]。字符比较次数为h[i]+1，不超过h[i]-h[i-1]+2。否则，说明i>1，Rank[i]>1，h[i-1]>1，根据性质3，Suffix(i)和Suffix(Rank[i]-1)至少有前h[i-1]-1个字符是相同的，于是字符比较可以从h[i-1]开始，直到某个字符不相同，由此计算出h[i]。字符比较次数为h[i]-h[i-1]+2。 　　
不难看出总的字符比较次数不超过3n,也就是说，整个算法的复杂度为O(n)。 　　求出了h数组，根据关系式height[i]=h[SA[i]]可以在O(n)时间内求出height数组，于是可以在O(n)时间内求出height数组。 　　
结合RMQ方法，在O(n)时间和空间进行预处理之后就能做到在常数时间内计算出对任意(i,j)计算出LCP(i,j)。 　　
因为lcp(Suffix(i),Suffix(j))=LCP(Rank[i],Rank[j])，所以我们也就可以在常数时间内求出S的任何两个后缀之间的最长公共前缀。这正是后缀数组能强有力地处理很多字符串问题的重要原因之一。   