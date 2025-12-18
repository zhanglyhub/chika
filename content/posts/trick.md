---
title: "KomariChika suki Data Structures"
date: 2025-12-19T15:30:00+08:00
draft: false
images:
  - "/images/f.jpg"
---
![test](/images/f.jpg)
1.给定序列$a,$选择任意区间$[l,r]$，将区间内数改为$l+r$，求$\max{\sum_{i=1}^na_i}$，$\forall\:[l,r]$都有，$\sum_{i=l}^r{l+r}=\sum_{i=l}^r2i$(可看作步长为二的等差数列)，构造$b_i=2i-a_i$，求$b$的最大子段和即可。[Problem - C - Codeforces](https://codeforces.com/contest/2169/problem/C)

2.好题，给定$x,y,k$，每次删去$m*y,m\in[1,2,3...]$，求$x$次迭代后的$k$小值。序列几乎被认为是无限的，正难则反，考虑一次操作后$k$会出现在哪里，显然有$k'=k-\lfloor\frac{k}{y}\rfloor$，$O(N)$的进行$x$轮迭代不足以通过此题，z这里可以进行变换$k'=k-\lfloor\frac{k-1}{y-1}\rfloor$，有点类似于数论分块的思想，将序列分为大小为$y$的块，每次删去第一个数，令$s=\lfloor\frac{k-1}{y-1}\rfloor$观察发现$s$不是每次在变的，考虑以$s$为步长最多迭代多少轮，显然有$\frac{k-1}{y-1}>s+1$，解得答案就是$\min(x,(s+1)*(y-1)-k)/s+1)$[Problem - D2 - Codeforces](https://codeforces.com/contest/2169/problem/D2)

3.划分可重集$a$，将其众数加入可重集$s$求最大划分方案。考虑一个数什么时候不会成为众数,$cnt_{x\notin S}<=\sum_{c\in S}cnt_c$，显然只要$maxcnt_{x\notin S}<=\sum_{c\in S}$，经典问题，选一些数不超过$maxcnt_{x\notin S}$的方案数，01背包即可[Problem - D - Codeforces](https://codeforces.com/contest/2166/problem/D)

关于在KomariChika的trick中学math这档事

4.统计好子序列的数量(定义具体看题)，首先这是一个组合数，发现直接不好算，$dp$状态数太多，考虑定义，任意一个个好子序列$T$，我们枚举$|\frac{T}{2}|$的位置，合法子序列必然一部分出现在$|\frac{T}{2}|$的前面和后边，设$T$的长度为$k$，$S$的前$|\frac{T}{2}|$有$p$个字符后有$q$个字符则有$\sum\limits_{k>=1}\binom{p}{k-1}\binom{q}{k}$[F - 1122 Subsequence 2](https://atcoder.jp/contests/abc433/tasks/abc433_f)复杂度$O(|S|)$

$$
\sum\limits_{k>=1}\binom{p}{k-1}\binom{q}{k}=\sum\limits_{k1+k2=q-1}\binom{p}{k_1}\binom{q}{k2}\\\\\\\\\\=[x^{q-1}]\sum_{k_1>=0}\binom{p}{k_1}x^{k_1}\sum_{k2>=0}\binom{q}{k_2}x^{k_2}\\\\\\\\\\\\\\\=[x^{q-1}](1+x)^{p}(1+x)^q=\binom{p+q}{q-1}=\binom{p+q}{p+1}
$$

$$

5.发现性质如何高效处理，这道题你发现这字符串肯定要错位匹配，思维不要固化，尝试正反思考，正向去模拟不好搞，你就反向匹配找最近的匹配字符，这样问题就简化了许多，特别注意题目所给的数据量信息，这里有$n*k_{max}x$显然要写与$k$有关的算法复杂度$O(nk_{max})$[Problem - 2164D - Codeforces](https://codeforces.com/problemset/problem/2164/D)

同样，找到题目数学关系时，不要着急去码，构建大致框架比如这题[D - 183183](https://atcoder.jp/contests/abc433/tasks/abc433_d)，一眼是这个$A*10^{len(b)}+B=0$被$m$整除，化简后可知$B\equiv-A*10^{len(b)}\:\pmod M$，你考虑二分，这里还是有点$trick$的，预处理$len(b)$和$B\:mod\:M$之间的映射，对于每个数去二分考虑(数位长度短所以这样搞)有没有答案，就清楚了很多。

6.给你一张图，求经过所有边回到出发点的最小花费，首先花费至少为$\sum\limits_{i=1}^{n}w_i$，考虑如何使传送花费最小化，定义传送的花费为$\min\limits_{i=u}^{v}w_i$，考虑什么情况才会使用传送，易想到欧拉路径，显然只有奇数度的边才会进行传送，问题转化为找到奇数度的点两两匹配使得花费最小，考虑对原图建出重构树，显然任意两个点之间的最小花费就是$\min\limits_{u,v\in \:odddegree}\sum\texttt{LCA}(u,v)$，显然在重构树中**任意一个父节点$x$个子节点$u,v$的$\texttt{LCA}$就是$x$**，要注意的是在建重构树的时候如果两个连通分量已经连通了我们要更新当前的最小权值而不应该简单跳过，

贪心匹配的时候如果两个左右子树都为奇数度点为偶数就地匹配，否则向上传递到父节点，要特别注意看清楚题意的**定义是值还是索引**。

7.傻逼题，but，并查集还能这么玩，维护区域矩阵连通性时考虑可以为每个行维护一个并查集实现快速跳转可以做到$O(nm)$，开数组时切记$n<=m$否则会爆。

8.肯定要按位来搞，相当于给每个数维护了一个集合，显然较大的数贡献更多，搞一个堆，每次从集合中取数即可。[Problem - 2165C - Codeforces](https://codeforces.com/problemset/problem/2165/C)

9.每次可以给序列单点加1整体乘2求最少操作次数使得序列$a=b$，显然乘2是更优的，显然最大乘二操作次数为$\min\limits_{i=1}^{n}\log_2\frac{b_i}{a_i}$，根据乘法分配律，显然把加1操作穿插到乘法之间是更好的，考虑第$k$次乘法操作，此时必然满足$a_i*2^k<=b_i$，考虑此时进行一次加法操作，相当于给序列多加了$2^k$，所以总共进行的加法操作为$\lfloor\frac{b_i-a_i*2^k}{2^k}\rfloor=\lfloor\frac{b_i}{2^k}\rfloor-a_i$，则此时我们距离$a_i$还有$b_i-a_i*2^k mod\:2^k=b_i mod\:2^k$，所以总共进行的加法操作为$\texttt{popcount}(b_i\:mod2^k)$，加起来即可。第二问求所以可能的方案数，设当前$a_i$进行了$c_i$次加法操作所以总可能数为$S!=(\sum\limits_{i=1}^{n}c_i)!$，但这样会算重，应为每个$a_i$位置进行的操作$c_i$本质上是没区别的给$(a_i,1)$和$(1,a_i)$没区别，所以要出他们的排列$\prod\limits_{i=1}^{n}c_i!$所以对于每个位置乘一个多项式系数即可$\frac{\sum\limits_{i=1}^{n}c_i)!}{\prod\limits_{i=1}^{n}c_i!}$。c

10.考虑$x+y=k$这条对角线，显然，任意一条从左上到右下的路径都是要经过这条对角线的，先预处理一个极左极右路径，极左路径不穿过障碍物的尽量靠左的合法路径，极右路径同理，任意合法路径必在这夹在两条路径之间，如果不存在显然答案就是$\binom{n*m}{2}-1$,

显然这两条路径的交是必```ban```的，只要```ban```掉其中一个点再随便搞一个都行，枚举这条极左路径上的每一个点，依次ban掉，计算新的路径极左路径，答案就是它和极右路径的交$O(nm)$。[Cells Blocking ](https://qoj.ac/contest/443/problem/833)

upd:

11.二分出最短时间，关键再于check函数，考虑关键点向上跳肯定是更优的，所以我们向上跳到时间范围内的最浅父节点将当前父节点标true表示当前父亲所管辖的子树都已经安排好了，跑一遍dfs，如果遇到标记直接返回，否则找是否存在一条路径从根节点到叶子节点。复杂度$O(n\log^2n)$.[P1084](https://www.luogu.com.cn/problem/P1084)

12.分治线性检测，固定x考虑，观察到这个和式$\sum\limits_{i=1}^{n}w_i|t-Y_i|$是一个线性下凸函数，直接判定$f(\frac{a+b}{2})==\frac{f(a)+f(b)}{2}$即可，不用想直接分治$a,b$，$O(n\log V)$.·

Thupc2026

A:神题我直接Orz，求$\max\limits_{i=l}^{r}\text{LCA}(a_i,u)$，

B:看一眼数据可以$O(nq)$的，考虑一个询问怎么算，如果一个串存在border当且仅当$\exist len\:s[l+len-1]=s[r-len+1]$，

你发现可以mix一下，具体的令$t=reverse(s)$，则有$s_lt_ls_{l+1}t_{l+1}...s_{r}t_{r}$，随便观察一下显然只需要统计前半部分的偶回文即可。

J:赛时看了一眼榜，过的人挺多的，想到了要判奇偶的，但是当时G给我心态调炸了然后.....首先$n$是偶数无解，数学归纳法：当$n=2$时，显然，任何正整数的平方是2的偶次幂，假设n=k-2时成立，考虑n=k时，反证法，假设是有解的，则有$a_i+a_{i+1}=2^{b_i}$，则有$a_{k-1}+a_k<=2a_k<2(a_1+a_k)$，$a_1+a_k<=2a_k<2(a_{k-1}+a_k)$，则有，$2^{b_i-1}<2^{b_k+1},2^{b_k}<2^{b_{k-1}+1} \to b_k=b_{k-1},a_1=a_{k-1}$.所以$\prod\limits_{i=1}^{k-2}(a_i+a_{i+1})=\prod\limits_{i=1}^{k-3}(a_i+a_{i+1})(a_{k-2}+a_1)=\frac{\prod\limits_{i=1}^{k}(a_i+a_{i+1})(a_1+a_{k-2})}{(a_1+a_k)(a_{k-2}+a_{k-1})(a_k+a_{k-1})}$,化简可得$2^{2t-2b_k+1}$显然，所以n等于偶数无解。

若n为奇数....没看懂.....不过为啥过那么多....