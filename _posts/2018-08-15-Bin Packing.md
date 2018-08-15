---
layout:     post                    # 使用的布局
title:      近似算法：装箱问题              # 标题 
subtitle:   装箱问题的几种经典近似算法
date:       2018-08-15              # 时间
author:     Charles Xu              # 作者
header-img: img/post-bg-dog-04.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
mathjax: true
tags:                               #标签

- graph theroy
- minimum spanning tree

---
<script type="text/x-mathjax-config"> MathJax.Hub.Config({ tex2jax: {inlineMath: [['$$','$$'],['$','$'],['\\(','\\)']]} }); </script> <script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script> 

## Bin Packing Problem

### 问题的定义

> In the bin packing problem, objects of different volumes must be packed into a finite number of bins or containers each of volume *V* in a way that minimizes the number of bins used. In [computational complexity theory](https://en.wikipedia.org/wiki/Computational_complexity_theory), it is a [combinatorial](https://en.wikipedia.org/wiki/Combinatorics) [NP-hard](https://en.wikipedia.org/wiki/NP-hard) problem.[[1\]](https://en.wikipedia.org/wiki/Bin_packing_problem#cite_note-1) The [decision problem](https://en.wikipedia.org/wiki/Decision_problem) (deciding if objects will fit into a specified number of bins) is [NP-complete](https://en.wikipedia.org/wiki/NP-complete).[[2\]](https://en.wikipedia.org/wiki/Bin_packing_problem#cite_note-2).

最直观的一维装箱问题就是锯钢条，需要一些长度的钢条，这些钢条需要从标准长度的钢条中截取，作为大国工匠如何节约材料，使用最少的标准钢条达到需求？有经验的师傅常常能使得钢条的浪费较少，但是他们的解也不一定是最优解，因为这个问题是一个典型的 **NP-难** 问题。

### 模型的建立

设：有一堆物品长度分别为 $w_{i} $，若干长度为 $C (C>w_i)$ 的箱子；问：最少需要几个箱子才能把所有物品全部装进箱子？我们首先对问题进行一个简单的分析，回答两个简单的问题。

- 最坏的情况是什么？

  因为物品的长度小于箱子的长度，所以不存在一件物品一个箱子装不下的情况。因此，最坏的情况就是一个物品放一个箱子，所以可行解的上界是物品的个数 n。

- 最理想的情况是什么？

  最理想的情况就是每个箱子都填满，我们假设物品可以分割，那么每个箱子都可以被填满，在这种情况下所需要的箱子数应该为 $\lceil \frac{\sum w_i}{C} \rceil$ 。

综上所述，$OPT$ 的取值范围应当为 $[\lceil \frac{\sum w_i}{C} \rceil, n]$。下面给出几个变量的定义:
$$
y_i= \begin{cases} 1 & , if\ bin\ i\ is\ used. \\ 0 &, otherwise. \end{cases}  i=1, 2, \dots, n 
$$
$$
x_{ij} = \begin{cases} 1 &, if\ the\ item\ j\ is\ placed\ into\ bin\ i. \\ 0, & otherwise. \end{cases} i, j = 1,2,\dots, n
$$

问题的数学描述：

---

$$
\min z = \sum_{i=1}^{n}y_i \\ \begin{gather} s.t.\ \sum_{i-1}^{n} w_ix_{ij} \le C  y_i,& i = 1,2,\dots,n \tag{1} \\ \sum_{i=1}^{n}x_{ij}=1,& j = 1,2,\dots,n \tag{2}\\ y_i \in \{0, 1\},& i = 1,2,\dots,n \\ x_{ij} \in \{0, 1\},& i,j = 1,2,\dots,n \end{gather}
$$

---

表达成数学形式后，我们不难看出问题变成了整数规划问题，问题的目标是尽可能少的使用箱子；第一个限制条件表明每一个箱子中放置物品的总长度小于箱子长度 $C$；第二个限制条件表明，每个物品在所有箱子中能且仅能放置一次。

经过前面的课程，我们在解决整数规划问题时，有了一些经验，比如对问题进行松弛变成对应的线性规划问题，那么问题变成如下形式：

---

$$
\min z = \sum_{i=1}^{n}y_i \\ \begin{gather} s.t.\ \sum_{i-1}^{n} w_ix_{ij} \le C  y_i,& i = 1,2,\dots,n \tag{1} \\ \sum_{i=1}^{n}x_{ij}=1,& j = 1,2,\dots,n \tag{2}\\ 0 \le y_i  \le 1,& i = 1,2,\dots,n \\ 0 \le  x_{ij} \le  1,& i,j = 1,2,\dots,n \end{gather}
$$

LP 问题能在多项式时间内求出解，那么这个解的性质究竟如何呢？课程中给出了一个特例：共有 5 件物品，每件物品的长度均为 0.6，箱子的长度为1，那么显然当 $x_{ij} = 1/3$ 时，对应的解为 3，但是显然这个问题的最优解是 5，因此简单的松弛是无效的。

### Approximation Algorithms

#### Next-Fit Algorithm

NF 算法是最简单的装箱问题近似算法，按照物品给定的顺序进行装箱，时间复杂度为 $O(n)$，近似比为 2。。它的原则是：如果物品 j 能够放进第 i 个箱子，那么放进去；如果空间不够了，那么封闭第 i 个箱子，使用第 i+1 个箱子，将物品 j 放入。

> **Theorem**：$R_{NF} = 2$

上述定理可以利用反证法证明，我们假设 $OPT(I) = k,\ NF(I) > 2k$。显然，$kC \ge \sum w_ii$；又因为物品 j 是因为放不进箱子 i，才放进箱子 i+1 中的，所以两个箱子中的物品大小之和必定超过 C，于是前 2k 个箱子中的物品大小之和必定大于 kC，这就形成了矛盾，所以定理得证。

> 启示：
>
> 1、NF 算法获得了不错的效果这告诉我们，首先需要尝试最简单的算法；
>
> 2、我们有时候需要从直观感觉出发，分析一些具体的例子；

#### Fisrst-Fit Algorithm

FF 算法也是按照物品给定的顺序装箱，并且把物品放入到第一个适合它的箱子里面，时间复杂度为 $O(nlogn)$，近似比为 7/4。证明过程两本书都坦言复杂，因此基础学习阶段我也跳过吧。

> **Theorem**：$R_{FF} = 7/4$

#### First-Fit-Decreasing Algorithm

FFD 算法的第一步是把物品按照大小从大到小排序，然后按照 FF 算法进行装箱，时间复杂度为 $O(nlogn)$，近似比为 3/2。

> **Theorem**：R_{FFD} = 3/2

对于任意的实例 $I$ 有 $FFD(I) \ge OPT(I)$，记 $FFD(I) = l, OPT(l) = l\ast$。

- 结论一：FFD 算法所用的箱子从第 $l\ast + 1$ 开始到 $l$ 中每个物品的长度不超过  $\frac{1}{3}C$

  记 $a_i$ 是放入第 $l\ast + 1$ 个箱子中的第一个物品，即证 $a_i \le \frac{C}{3}$。反证法，如果 $a_i > \frac{C}{3}$，因为是从大到小排序，所以有 $w_1, w_2,\dots,w_{i-1} > \frac{1}{3}C$，如此前面每个箱子最多有两件物品。存在 k ≥ 0 使得前 k 个箱子只有一个物品，后面 $l\ast - k$ 个箱子恰含两个物品。我们假设有这样两个箱子 $B_p$ 和 $B_{p\prime}$，且 $p < p\prime$ ，$B_p$ 中两件物品 $w_{t1}, w_{t2} (t2 > t1)$ ，$B_{p\prime}$ 中有一个物品 $w_{t3}$，因为排过序，所以 $w_{t1} > w_{t3}，w_{t2} > w_{i}，w_{t1}+w_{t2} \ge w_{t_3} + w_i$，结果就是 $w_i$ 可以放进去啊，矛盾！因此 k 这个天然分界线是存在滴。为什么不把 $w_k, w_{k+1},\dots, w_i$ 放到前 k 个箱子，很显然前 k 个箱子的空间已经容不下它们了，在最优解中同样存在一个 k 满足前 k 个箱子无法放入 $w_k, w_{k+1},\dots, w_i$ ，于是他们只能加入第 $k+1,\dots, l*$ 等箱子中，但是后面的箱子中已经有两个物品无法放入第三个物品了，产生矛盾，因此假设不成立，结论一得证。

- 结论二：FFD 算法放入的箱子从 第 $l\ast + 1$ 开始到 $l$ 中的物品数不超过 $l\ast - 1$

  1. $\sum_{i=1}^{n} w_i < l* C$
  2. 假设有 $l*$ 个物品放进了 $l* + 1,\dots,l$ 个箱子中，记这些箱子中的前 $l*$ 个物品为 $a_i, \dots, a_{l*}$ 
  3. 记 FFD 算法前 $l*$ 个箱子每个相中物品总长度为 $b_j，j=1,2,\dots,l*$ 
  4. $\forall j = 1,2,\dots,l*， b_j + a_j>C$
  5. 综上：$\sum_{i=1}^{n} \ge \sum_{j=1}^{l*}b_j + \sum_{j=1}^{l*} = \sum_{j=1}^{l*}(b_j + a_j) > l* \cdot C$ 
  6. 上式中已经产生矛盾，所以结论二成立

- 因为结论一和结论二成立，所以后 $l-l*$ 个箱子是用来放至多 $l*-1$ 个物品的，而且·这些物品的大小都不超过 $\frac{1}{3}C$，因此最多再使用 $\lceil \frac{OPT(I)-1}{3} \rceil$ 个箱子，因此得到下式：
  $$
  \frac{FFD(I)}{OPT(I)} \le \frac{OPT(I)+\lceil \frac{OPT(I)-1}{3} \rceil}{OPT(I)} \le 1 + \frac{OPT(I)+1}{3OPT(I)} = \frac{4}{3} + \frac{1}{3OPT(I)}
  $$

  - case 1：当 $OPT(I) = 1$ 时，$FFD(I)$ 也应该为 1；
  - case 2：当 $OPT(I) \ge 2$ 时，$\frac{FFD(I)}{OPT(I)} \le \frac{4}{3} + \frac{1}{6} = \frac{3}{2}$

  因此，FFD 近似比是 $3/2$。

### Some Cases in Class

#### Special Case: Item sizes are all small than $\epsilon \times C$

我们假设所有待放置物品的大小不超过箱子大小的 1/3，那么除最后一个箱子外，每个箱子被封闭时，显然所装物品大小之和超过箱子大小的  2/3。因此，我们可以得到这样的结论：$OPT(I) \ge \sum w_{ij} \ge \frac{2}{3}(A(I) - 1) $，进而得到 $A(I) < \frac{3}{2}OPT(I) + 1$。式中 $A(I)$ 表示基于输入和某一种近似算法得出的结果。换言之，如果我们能够保证每件物品的大小不超过 $\epsilon \times C$，那么我们就能保证最终的解满足 $A(I) < \frac{1}{\epsilon}OPT(I) + 1$。

#### Special Special Case: Item sizes are large but they in some distinct values.

在这个特例中我们假设物品的大小都比较大，但是它们的大小限定为几个值之中。在课程之中，老师引出了 *configurations* 的概念。其实，就是几个物品大小值的组合，因为物品大小的可选值并不多，所以 *configurations* 的大小也不会很大，最终我们从中挑选一些设置成为输出集合为 $C = \{configurations\}$，$a_{s,c}$ 表示大小 s 在某个设置 c 中出现的次数，比如一个为【3，4，4】的设置，$a_{3,c} = 1, a_{4,c} = 2 $。这样一来装箱问题就又回到了整数规划问题，问题的定义如下：

---

**Integer Program:**

1. Input : S = {size}, number of size s: $n_s$
2. Outpit: $C = \{configurations\}$, number of bins in configuration C:$x_c$
3. Constraints: $\sum_{C} a_{s, c}x_c \ge n_s$
4. Number of bins: $\sum_{C}x_C$

---

我们首先基于物品的大小，求出它们互相搭配不同的组合，然后就是一个整数规划问题，设置集合中每种设置挑选几个能够满足每种大小物品出现的次数问题。如果每个物品的大小都大于箱子大小的 1/10 ，那么每个设置中最多有 10 个物品；进一步假设物品大小的种类小于 10 的话，那么设置的总数量不会超过 100，原问题变成了一个包含 10 个限制条件，100 个变量的线性规划问题，然后把结果近似到最近的整数，那么得出的解不会超过 $OPT(I) + 100$（如果每个值都大了1的话，就多了100个）。

#### Less Special Case: Large items in many sizes

在上面的“特特例”中，我们做了一个限制：东西可以很大，但是不同大小的数目要少。在本例中，我们拿掉了对于大小种类的限制。如果没有大小种类的限制，那显然会有很多很多大小的数值，换一个直觉上的操作就是 **Round** , 那我们该如何取整呢？第一个想法是：Round sizes up to the nearest integer。这么做到底好不好？我们假设物品最小大于箱子大小的 1/10，那么取整可能的值有 90 个，另外我们举个特例，50 个物品大小为 100/3 和 50 个物品大小为 200/3，我们一眼可以看出刚刚好50个箱子，但是如果我们取整变为 34 和 67，最优解就变成了 75，很显然这个结果很坑。

上面的 Round 规则不行，有没有行的规则呢？有，那就是 Adaptive Round。其主要思想就是将值区间划分为很多等间距的子区间，落在同一区间内的值全部变为区间内的最大值。想一想，还是上个图，比较直观一点。

![](https://ws2.sinaimg.cn/large/0069RVTdgy1fual8yetuoj30xr0jbq4j.jpg) 

---

**Algorithm-large items**

1. Assume: sizes > capacity * $\epsilon$
2. Sort sizes
3. Make groups of cardinality n × $\epsilon ^ 2$, get $1/\epsilon$ groups
4. Round up to max size in group
5. Solve rounded problem
6. Output corresponding packing

---

**分析：** 这种方法到底好不好？有多好？

如果有 n 个物品，每个物品的大小不小于箱子大小的 $1/\epsilon$，那么由我们开始的分析可知，需要箱子数目的下界为 $\epsilon n$，因此 $n\epsilon^2 \le \epsilon \times (n\epsilon) \le \epsilon OPT$。

> **Theorem:** When all sizes are > $\epsilon B$ algorithm, in polynomial time gives packing s.t. Value(Output) < OPT * (1+ $\epsilon$)

#### General Case

---

**Algorithm-General algorithm**

1. Set aside: sizes < capacity * $\epsilon$
2. Sort remianing sizes
3. Make groups of cardinality n × $\epsilon ^ 2$, get $1/\epsilon$ groups
4. Round up to max size in group
5. Solve rounded problem U
6. Greedily add small sizes

---

**分析：**物品由两部分组成，一部分为体积小的物品集 S，一部分是体积较大的物品集 L，$I = S \cup L$。

- case 1：在将 S 加入的过程中没有添加新的箱子；

  $Value(Output) = Value(packing\ of\ L) ≤ (1+\epsilon) \cdot OPT(L) ≤ (1+\epsilon)  \cdot OPT(I)$

- case 2：在将 S 加入的过程中添加了新的箱子，除了最后一个箱子都都能达到 $(1- \epsilon)$ 的装载率；

  $\sum w_i \ge (K - 1)(1- \epsilon)$

  $\sum w_i \le OPT$

  $Value(Output) \le \frac{1}{1-\epsilon}OPT + 1$

### References

1. 维基百科：[Bin Packing Problem](https://en.wikipedia.org/wiki/Bin_packing_problem)；
2. 《数学规划与组合优化》第 10 章：装箱与平型机排序问题；
3. 《Combinatorial Optimization: Theory and Algorithms》第 18 章：Bin-Packing；
4. 《Approximation Algorithms》第 9 章 BIn Packing；
5. Coursera 课程 Approximation Algorithm Part 1 第三周课程 [Bin-Packing](https://www.coursera.org/learn/approximation-algorithms-part-1/lecture/vAkWL/lecture-next-fit)；

