layout:     post                    # 使用的布局
title:      近似算法：背包问题 			   # 标题 
subtitle:   0-1背包问题近似算法
date:       2018-08-11              # 时间
author:     Charles Xu              # 作者
header-img: img/post-bg-pen-0.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
mathjax: true
tags:                               #标签

- approximation algorithm
- knapsack problem 

#### Knapsack Problem

> **Definition:** The problem restricts the number *xi* of copies of each kind of item to zero or one. Given a set of *n* items numbered from 1 up to *n*, each with a weight *wi* and a value *vi*, along with a maximum weight capacity *W*,
>
> $$maximize \sum_{i=1}^{n}v_ix_i$$
>
> $$s.t. \sum_{i=1}^{n}w_ix_i \le W\  and\ x_i \in \{0, 1\} $$

#### Subset Sum Problem

> **Definition:** The problem is this: given a set of integers $$S=\{x_1,x_2,\dots,x_n\}$$ and a value t, is there a non-empty subset S' of S whose sum is v?
>
> $$\exist S' \in S: \sum_{x \in S'}x = t $$

今天，听了Coursera上近似算法课程第一部分的第二周内容 **Knapsack and Rounding**。课程中讲述了 **0-1背包问题 **基于贪心算法，DP算法和近似算法的三种解法，中间有些许内容未能在上课时消化，于是进一步查阅资料后做此梳理。

教程中讲述的是0-1背包问题的一个特例，即 $$w_i = v_i$$ 时的情况，而满足这个条件的时候，0-1背包问题又可以演化出上述的另一个问题——**子集和问题**。当然，这两个问题不是完全一样，后面阐述这类特殊背包问题的时候会详细分析。

我们首先分析子集和问题，要解决这个问题，一个最朴素的想法是考虑每个元素只有取和不取两种情况，然后再考虑它们组成子集的和是否等于M，这样的情况总共有 ***2^n*** 种，显然在 n 比较大时这样的算法的运行时间将难以接受。接着我们考虑用 DP 算法解决这个问题，我们用 **A(i, v)** 来表示 S 中前 i 个元素子集和为 v 的情况。然后，我们考虑第 i 个数的放置情况：

> If s[i] > v:
>
> ​	Discard it
>
> Else:
>
> ​	If not:
>
> ​		A(i-1, v) = A(i, v)
>
> ​	If yes:
>
> ​		A(i-1, v-S[i]) = A(i, v)

同时，基于观察可知，$$\forall i \in \{0,1,\dots,n\}$$ ，A(i, 0) = 1,  $$\forall j \in \{0,1,\dots,M\}$$ ，A(0, j) = 0。然后可以，利用 DP 方法构建 n+1 行，m+1 列的真值表。当提供的整数个数为0时，除0外的任何情况 A[0, v] = 0, 当设置的子集和为0时，i为1~n间的任意值，A[i, 0] = 1。

```python
import numpy as np


def is_subset_sum(S, v):
    """
    :param S: the set of integers
    :param v: the target subset sum
    :return: None
    """
    n = len(S)
    m = v
    table = np.ones((n + 1, m + 1), dtype=int)

    # Initial the table for DP.
    # If no number is given, there is no satisfied subset.
    for i in range(0, n + 1):
        table[i, 0] = 1
    # If the
    for j in range(1, m + 1):
        table[0, j] = 0

    for i in range(1, n + 1):
        for j in range(1, m + 1):
            # If the ith number is larger than capacity j, the problem can be reduced to A[i-1, j]
            if j < S[i - 1]:
                table[i, j] = table[i - 1, j]
            else:
                table[i, j] = table[i - 1, j] or table[i - 1, j - S[i - 1]]

    if table[n, v]:
        res = list()
        i = n
        M = v
        while i:
            # If A[i, M] is true and A[i-1, M] is false, it means the i th number is necessary.
            if table[i, M] and not table[i-1, M]:
                res.append(S[i-1])
                M -= S[i-1]
            if M == 0:
                print("Congratulations, a satisfied subset is found:", end=' ')
                res.sort()
                for x in res:
                    print(x, end=' ')
                print()
                break
            i -= 1
    else:
        print("Sorry, no subset with the given sum value.")
```

经过上面的讨论，我们已经解决了子集和问题，那么当  $$w_i = v_i$$ 时，0-1 背包问题就是求一个不大于 W 的子集和问题。

#### Approximation Schemes

$$\Pi$$ ，表示一个 NP-hard 最优化问题；$$f_{\Pi}$$ ，表示目标函数；

$$\mathcal{A}$$ ，表示一个近似方案；$$I$$ ，表示问题输入；$$\epsilon$$ ，表示方案误差参数；s ，表述方案输出的一个结果；

如果满足以下两个条件，那么我们称 $$\mathcal{A}$$ 是一个 *polynomial time approximation scheme (PTAS) *。

![](https://ws1.sinaimg.cn/large/006tNbRwgy1fu5tf4xoghj314e04lgln.jpg)

##### **PTAS（Polynomial time approximation scheme）**

定义：若对于每一个固定的 $$\epsilon > 0$$ ，算法 $$\mathcal{A}$$ 的运行时间以实例 $$I$$ 的规模的多项式为上界，则称 $$\mathcal{A}$$ 是一个多项时间近似方案。

##### **FPTAS（Fully polynomial time approximation scheme）**

定义：在 PTAS 的基础上，进一步要求算法 $$\mathcal{A}$$ 的运行时间以实例 $$I$$ 的规模和 $$1/\epsilon$$ 的多项式为上界，则称 $$\mathcal{A}$$ 是一个完全多项时间近似方案。FPTAS 被认为是最值得研究的近似算法，仅有极少数的 NP-hard 问题存在 FPTAS。

##### **PPTAS（Pseudo-polynomial time approximation scheme）**

定义：如果算法的时间复杂度可以表示为输入数值规模 N 的多项式，但是运行时间与输入值规模 N 的二进制位数呈指数增长关系，则称其时间复杂度为伪多项式时间。一个具有伪多项式时间复杂度的NP完全问题称之为弱NP完全问题，而在 P!=NP 的情况下，若一个NP完全问题被证明没有伪多项式时间复杂度的解，则称之为强NP完全问题。

##### **0-1 背包问题的伪多项式时间算法**

$$S=\{a_1,\dots,a_n\}$$ 给定物体集合，$$size(a_i) \in \mathbb{Z}^+, profit(a_i) \in \mathbb{Z}^+, B \in \mathbb{Z}^+$$ ；

$$P = \max_{a_i \in S}profit(a_i)$$，则 nP 是所有解能够获得的一个平凡上界；

$$\forall i \in \{q,\dots,n\}\  $$，$$\forall p \in \{1,\dots,nP\}$$，$$S_{i,p}$$ 表示 $$\{a_1,\dots,a_i\}$$ 的一个子集，该子集的收益为 p, 并且总的 size 达到最小，设 $$A(i, p)$$ 表示集合 $$S_{i,p}$$ 总的 size 大小，如果不存在则为无穷大。显然，对所有的 $$p \in \{1,\dots,nP\}$$ ，$$A(1,p)$$ 是已知的。可以退推出递归的表达式：

$$A(i+1,p)=\begin{cases} min\{A(i, p),size(a_{i+1})+A(i,p-profit(a_{i+1}))\} & if\ profit(a_{i+1}) \le p \\ A(i, p)  & otherwise\end{cases} $$     

如此，总 size 大小以 B 为上界的个物体能够达到的最大收益为 $$\max\{p|A(n,p) \le B\}$$，p的大小可能是指数级的，时间复杂度为O(n^2P)，n 个物品，每个物品对应 1~nP 取值不同的子问题。

##### **0-1 背包问题的 FPTAS**

设计思路：如果各个物品收益均是比较小的数，即是以 n 的多项式为上界， 那么基于上述递推式设计的 DP 算法是一个常规多项式时间的算法，因为其运算时间以 $$|I|$$ 的多项式为上界，但是当 p 为指数级时，就需要对物品价值进行放缩，**即忽略物品价值的最后若干有效位（依赖于误差 $$\epsilon$$ )**，以便将修改后的收益看作是以 n 和 $$1/\epsilon$$ 的多项式为上界，最终在以 n 和  $$1/\epsilon$$ 的多项式为上界的时间内找到收益至少为 (1- $$\epsilon$$) *OPT的解。

Scaling and Rounding:

> - Step 1: 给定 $$\epsilon > 0$$，设 $$K = \frac{\epsilon P}{n}$$；
> - Step 2: 对每个物品 $$a_i$$，定义 $$profit'(a_i) = \lfloor \frac{profit(a_i)}{K} \rfloor$$；
> - Step 3: 用这些值作为物品新的收益，利用 DP 算法找到最大收益的组合，记为 S ；
> - Step 4: 输出 S ；
>

原来问题的时间复杂度是 O(n\*nP)，进行放缩之后 P’ = P/K = n / $$\epsilon$$，因此输入变化之后问题的时间复杂度变为了 O(n\*nP') = O(n^3 / $$\epsilon$$ )，整体上讲属于 cubic time 的复杂度。

分析完问题的时间复杂度，下面分析该近似算法的好坏。用 S* 表示放缩后未取整之前最优的物品集合（放缩不会影响最优解的结构），即 OPT 对应的解。

> $$\sum_{S*} profit'(a) \le \sum_{S} profit'(a)$$                # 基于修改后的输入，S是最优输出
>
> $$\sum_{S*}profit'(a) > \sum_{S*}profit(a) - n$$        # 取整后的价值大于原价值 - 1
>
> $$\Rightarrow Value(S) \ge \frac{1}{K} \sum_{S}profit'(a)$$
>
> $$\Rightarrow Value(S) \ge \frac{1}{K} \sum_{S*}profit'(a)$$ 
>
>  $$\Rightarrow Value(S) \ge \frac{1}{K} (\sum_{S}profit'(a) - n) = OPT - \frac{n}{K}$$ 

#### 参考文献

1. Coursera巴黎大学 [近似算法课程](https://www.coursera.org/learn/approximation-algorithms-part-1/home/week/2)
2. 动态规划法（三）[子集和问题](https://segmentfault.com/a/1190000015115623)
3. [A Brief Introduction to the Approximation Algorithm](http://home.ustc.edu.cn/~huang83/alg/alg5.pdf)
4. The Knapsack Problem and Fullly Polynomial Time Approximation Schemes, Katherine Lai, Seminar in Theoretical Computer Science, March 10, 2006

