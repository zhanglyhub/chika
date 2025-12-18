---
title: "KomariChika suki Data Structures"
date: 2025-12-18T15:30:00+08:00
draft: false
images:
  - "/images/c.jpg"
---
![test](/images/c.jpg)
Suffix Automaton[G - Substring Game](https://atcoder.jp/contests/abc433/tasks/abc433_g)AC自动机[P2292](https://www.luogu.com.cn/problem/P2292)

A new way to maintain interval by bitset

1.it maybe unless...when range is small,we can use square root decomposition on with bitset.For all query in $l,r$ ,maintain a answer bitset.if they are in the same block,use brute force ,else preform an $or$ operation on the block and answer.

2.可持久化并查集，大概......考虑为每个格子维护建节点，维护5个指针指向上下左右上一次更新的数值，维护集合$S(v)$表示以$v$为原点最大拓展的区域，将值$v$相同合并，显然答案就是集合数量，对于一次更新，新建一个节点，继承之前上下左右指针指向的信息$O(1)$，之后查新节点的上下左右是否合法如果他大就把他标为不合法，如果他的邻居大就把他的邻居标为不合法，如果相等就合并$O((nm+q)*\alpha(nm+q))$.

3.单点修改区间修改区间子序列异或最大值查询。令$b_i=a_i\oplus a_{i-1}$，线段树维护$b$序列的线性基，对于区间修改相当于$b_l,b_{r+1}$两次单点修改，考虑区间查询，显然$\max\oplus_{i=l}^{r}a_i=\max a_l\oplus b[l+1...r]$，再将$a_l$插入即可，复杂度$O((n+q\log n)\cdot m^2)$

[P11731](https://www.luogu.com.cn/problem/P11731) [P11620](https://www.luogu.com.cn/problem/P11620)，其实如果不带修可以考虑分治处理$[l...mid]$的后缀线性基，处理$[mid+1...r]$的前缀线性基

upd1:考虑令$b_i=a_i\oplus a_{i-1}$，则有$a_{l+1}=a_l\oplus b_{l+1}\to \text{span}\{a_l,a_{l+1}\}=\text{span}\{a_l,a_l\oplus b_{l+1}\}\to\text{span}\{a_l,b_{l+1}\}$ 

所以对于区修改为点修，查询$\max\oplus_{i=l}^{r}a_i=a_l\cup b_{l+1...r}$，线段树维护差分值，树状数组维护前缀，复杂度$O(\log n\log^2V)$.感觉悬啊，小优化：满秩的时候不用插入直接返回.(不过好像没啥用还跑的更慢了).

upd2:想了好久都没有想到如何维护区间赋值，这大概需要ODT的思想？肯定是要离线下来的，线段树分治加到时间轴上，目前做到：$O(\frac{\log^3V}{w})$，区间赋值只能做到$O(\frac{nmq}{w})$.

4.分治预处理$i<j\land a_i<=a_j$的数量维护维护全局BIT，分治时找到答案就再BIT当前位置加1，query的时候直接query(r)-query(l-1);复杂度$O(n\log^2 n+q\log n)$ [Ynoi2005](https://www.luogu.com.cn/problem/P7906)

5.首先很容易有一个$O(nq\log n)$的分治，好像过不了，考虑优化仔细观察，一个查询区间$[l...r]$，一定会被这些小的分治区间覆盖，联想分块的思想，对于那些完整的分治区间(查询区间被整块包裹)，我们经行预处理计算维护$f_j=\max\limits_{j<i}\{f_i,a_i\}$,单调队列表都行，对于散块$O(n)$暴力，总复杂度$O(nq+n\log^2n)$。

upd:想了好久，还是不够深刻，考虑我们需要维护什么，有点想不明白这个倍增区间的样子，写了个分治预处理维护左右儿子的最优解，但是区间怎么锁定在那一块呢？还有数字组是炸空间的，？

upd:不会，想不明白这个，暂时放弃。

6.区间加，查询$\forall(i,j),a_p>=y\land t_j<=t_i$，显然不能cdq这带修改，既然不强制在线，考虑分治，具体的，建值域线段树，时间线线段树，借用扫描线的思想，$dfs$时间线段树，对于区间修改，直接在原线段树上搞$O(\log n)$，对于查询操作这里我刚开始想的是stack，记一下历史值但是这是朴素的，只能做到$O(q^2\log n\log q)$，珂朵莉告诉你这可以线段树二分，于是你想了想，便有了接下来优秀的$O(q\log q\log n)$的解法，发现可以直接time segment tree 维护$[l...r]$时间内的最小值，接下来，搜左右子树即可。[P3863](https://www.luogu.com.cn/problem/P3863)

其实远不及lxl的分块......
