---

layout:     post                    # 使用的布局
title:      图论：最小生成树              # 标题 
subtitle:   最小生成树的三种经典解法
date:       2018-08-12              # 时间
author:     Charles Xu              # 作者
header-img: img/post-bg-dog-1.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
mathjax: true
tags:                               #标签

- graph theroy
- minimum spanning tree

---
<script type="text/x-mathjax-config"> MathJax.Hub.Config({ tex2jax: {inlineMath: [['$$','$$'],['\\(','\\)']]} }); </script> <script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script> 

## Basic Knowledge

**树（Tree）：** 如果无向连通图中不存在回路，则这种图称为树。

**森林（Forest）：** 如果无向图中包含了几棵树，那么该无向图可以称为森林，森林为非连通图。

**生成树（Spanning Tree）：**如果无向连通图 $G$ 的子图是一棵包含了 $G$ 的所有顶点的树，那么该子图叫做 $G$ 的生成树，生成树是连通图的极小连通子图，加一条边就会构成回路，减一条边，就会变成非连通图； 

**最小生成树（Minimum Spanning Tree）：** 

> A **minimum spanning tree** (**MST**) or **minimum weight spanning tree** is a subset of the edges of a [connected](https://en.wikipedia.org/wiki/Connected_graph), edge-weighted (un)directed graph that connects all the [vertices](https://en.wikipedia.org/wiki/Vertex_(graph_theory)) together, without any cycles and with the minimum possible total edge weight. 

**并查集（Union-find Set）：** 是一种树型的数据结构，用于处理一些[不交集](https://zh.wikipedia.org/wiki/%E4%B8%8D%E4%BA%A4%E9%9B%86)（Disjoint Sets）的合并及查询问题。有一个联合-查找算法定义了两个用于此数据结构的操作：

- Find：确定元素属于哪一个子集。它可以被用来确定两个元素是否属于同一子集。
- Union：将两个子集合并成同一个集合。

```python
class UFset:
    """
    Union-find class
    """

    def __init__(self, n):
        self.n = n
        self.parent = [-1 for i in range(0, n + 1)]

    def find(self, x):
        p = x
        while self.parent[p] >= 0:
            p = self.parent[p]

        while x != p:
            t = self.parent[x]
            self.parent[x] = p
            x = t
        return p

    def union(self, n, m):
        r1 = self.find(n)
        r2 = self.find(m)
        t = self.parent[r1] + self.parent[r2]
        if self.parent[r1] > self.parent[r2]:
            self.parent[r1] = r2
            self.parent[r2] = t
        else:
            self.parent[r2] = r1
            self.parent[r1] = t

    def print(self):
        for i in range(1, self.n + 1):
            print(self.parent[i], end=' ')

```

**等价类（Equivalent Class）：** 在数学中，假设在集合 $X$ 上的一个等价关系（~），则 $X$ 中的某个元素 a 的等价类就是在 $X$ 中等价于 a 的所有元素形成的子集。

------



## Minimum Undirected Spanning Tree

**问题定义：**

图 $$G = (V, E, w)$$ 是一个带权连通无向图，$$w : E \rightarrow \mathbb{R}$$ 是定义在边上的权重。我们希望找到图 $G$ 

的一个一个无环子集 $T \in E$ ，既能够连接所有的顶点，又具有最小的权重，即 $w(T) = \sum_{(u,v) \in T}w(u, v)$ 的值最小。由于 $T$ 是无环的，并且连通所有的顶点，所以 $T$ 必然是一棵树。我们把这样的树称之为图 $G$ 的最小生成树。

**构造生成树的原则：**

1. 必须使用图中的边来构造；
2. 必须使用且仅能使用 n-1 条边来连接图中 n 个顶点；
3. 不能使用产生回路的边；

**生成树的构建：**

> GENERIC-MST(G, w)
>
> 1	A = ∅
>
> 2	while A does not form a spanning tree
>
> 3		find an edge(u, v) that is safe for A
>
> 4		A = A ∪ {(u, v)}
>
> 5	return A

在上述过程中，构造 $$MST$$ 的过程，就是不断寻找 $A$ 的安全边的过程。而 $A$ 被包含在图的某一棵 $MST$ 中。那么究竟什么样的边，对于 $A$ 来说是一条 **safe edge** 呢？通俗理解：就是这条边加入 $A$ 后还要能保证其是图的某一棵 $MST$ 一个子集。下面，给出一些定义:

- $(S, V-S)$ ，是图 $G$ 顶点集合 $V$ 的一个划分；
- $(u, v)$ 横跨切割 $(S, V-S)$，当且仅当 $(u, v) \in E$，并且两个端点分别属于两个切割的两个集合；
- 切割尊重集合 $A$ ，说明 $A$ 中不存在横跨切割的边；
- 轻量级边，在横跨切割的所有边中权重最小的那一条边，不一定唯一；

> **定理：**  $A$ 是边的集合，且被包含在 $G$ 的某一棵 $MST$ 中；$(S, V-S)$ 是图 $G$ 顶点集合 $V$ 的一个切割，并且其尊重 $A$；边 $(u, v)$ 是横跨切割 $(S, V-S)$ 的一条轻量级边；那么， $(u, v)$ 对于 $A$ 是安全的。
>

下面讨论的一些经典算法实际上都是寻找安全边的不同策略，这些算法的核心思想都是贪心策略，$MST$ 问题也是利用贪心策略能够获得最优解的特例之一。

### Classic Algorithms

#### Kruskal

**思路：** 寻找安全边的方法是，在所有连接森林中两棵不同树的边里面，寻找最小的边 $(u, v)$。

> MST-KRUSKAL(G, w)
>
> 1	A = ∅
>
> 2	for each vertex v ∈ G.V
>
> 3		MAKE-SET(v)
>
> 4	sort the edges of G.E into nodecreasing order by weight w
>
> 5 	for each edge (u, v) ∈ G.E, take in nodecreasing order by weight
>
> 6		if FIND-SET(u) ≠ FIND-SET(v)
>
> 7			A = A ∪ {(u, v)}
>
> 8			UNION(u, v)
>
> 9	return A 

**复杂度：** 时间复杂度为 O(|E|lg|E|)，主要取决于边数，适合稀疏图。

**图例：** 

![](https://ws3.sinaimg.cn/large/0069RVTdgy1fu6ysnifpjj30n50ftjs1.jpg)

**代码：**

```python
from collections import namedtuple

def Kruskal():
    mst_weight = 0
    selected_edge_num = 0
    V = set()
    E = list()
    Edge = namedtuple('Edge', ['u', 'v', 'w'])
    with open('2.15') as fp:
        for line in fp.readlines():
            u, v, w = [int(x) for x in line.split()]
            E.append(Edge(u, v, w))
            V.add(u)
            V.add(v)
    # Sort edges by weight.
    E = sorted(E, key=lambda x: x.w)
    # Use the union-find set to check weather the new edge will lead to cycle.
    c = UFset(len(V))
    for (u, v, w) in E:
        if c.find(u) != c.find(v):
            print(u, v, w)
            mst_weight += w
            c.union(u, v)
            selected_edge_num += 1
            if selected_edge_num == len(V) - 1:
                break
    print("The weight of MST is:", mst_weight)

```



#### **Prim**

**思路：** Prim 算法具有一个性质是集合 $A$ 中的边总是构成一棵树。这棵树从某一顶点 r 开始，每一次扩展所加入的边必须是使得树的总权重增加最少的边，最终形成图的一棵最小生成树。

> MST-PRIM(G, w)
>
> 1	for each vertex u ∈ G.V
>
> 2		u:key = ∞	# 连接该顶点和树中顶点的最小权重，如果不存在为无穷大
>
> 3		u:π = NIL	# 顶点在树中的父节点
>
> 4	r.key = 0
>
> 5 	Q = G.V
>
> 6	while Q ≠ ∅
>
> 7		u = EXTRACT-MIN(Q)	# 轻量级边在Q中的端点
>
> 8		for each v ∈ G.Adj[u]	# 更新与 u 相邻的Q中顶点的信息
>
> 9			if v ∈ Q and w(u, v) < v.key 
>
> 10				v.key = w(u, v)
>
> 11				v.π = u

**复杂度：** 时间复杂度为 O(|V|^2)，与图中的边数无关，适合稠密图。

**图例：**

![](https://ws2.sinaimg.cn/large/0069RVTdgy1fu6yziyd4rj30n50ftjs1.jpg)

**代码：**

```python
def Prim():
    mst_weight = 0
    V = set()
    E = list()
    Edge = namedtuple('Edge', ['u', 'v', 'w'])
    with open('2.15') as fp:
        for line in fp.readlines():
            u, v, w = [int(x) for x in line.split()]
            E.append(Edge(u, v, w))
            V.add(u)
            V.add(v)

    is_used = {x: 0 for x in E}
    c = UFset(len(V))
    Q = set()
    root = V.pop()
    V.add(root)
    Q.add(root)
    while Q != V:
        selected_edge = None
        selected_edge_weight = 65536
        for edge in E:
            if is_used[edge] == 0:
                u, v, w = edge
                if (c.find(u) == root and c.find(v) != root) or (c.find(v) == root and c.find(u) != root):
                    if w < selected_edge_weight:
                        selected_edge = edge
                        selected_edge_weight = edge.w

        if selected_edge:
            u, v, w = selected_edge
            print(u, v, w)
            c.union(u, v)
            mst_weight += w
            is_used[selected_edge] = 1

        for v in V:
            if c.find(v) == root:
                Q.add(v)

    print("The weight of MST is:", mst_weight)
    
```



#### Boruvka

**思路：** 该算法是最古老的 $MST$ 算法，其思想类似于 Kruskal 和 Prim。Boruvka 算法可以分为两步：1）对图中的各顶点，将与其关联的、具有最小权重的边选入 $MST$ ，得到 $MST$ 子树构成的森林；2）在途中陆续选择可以连接两棵不同子树，且具有最小权值的边将子树进行合并，最终构成单一连通分量 $MST$。  

> MST-BORUVKA(G, w)
>
> 1	Initial a forest T to be a set of one-vertex trees, one for each vertex of the graph
>
> 2	while T has more than one component:
>
> 3		for each component C of T:
>
> 4			begin with an empty set of edges S
>
> 5			for each vertex v in C:
>
> 6				find the cheapest edge from v to a vertex outside of C and add it to S
>
> 7		add the cheapest edge in S to T
>
> 8		combine trees connected by edges to form bigger components
>
> 9	Output: T is the minimum spanning tree of G

**复杂度：** 每一次循环都会将不连通分量的个数减半，一开始不连通分量的个数为 |V|，所以循环将要进行 log(|V|) 次。在每次循环的内部，需要检查每一条边，所以总的时间复杂度是 O(|E|lg|V|)。

**图例：** 

![](https://ws2.sinaimg.cn/large/0069RVTdgy1fu6yyf3514j30ny07st8w.jpg)

**代码：**

```python
from collections import namedtuple
def Boruvka():
    mst_weight = 0
    V = set()
    E = list()
    Edge = namedtuple('Edge', ['u', 'v', 'w'])
    with open('2.15') as fp:
        for line in fp.readlines():
            u, v, w = [int(x) for x in line.split()]
            E.append(Edge(u, v, w))
            V.add(u)
            V.add(v)
    # Use the union-find set to check weather the new edge will lead to cycle.
    c = UFset(len(V))

    Q = set()
    for v in V:
        Q.add(c.find(v))
    while len(Q) > 1:
        # In each loop, find the shortest edge of each set which links this set to another set.
        selected_edges = set()
        for n in Q:
            shortest_linked_edge = None
            shortest_linked_edge_length = 65536
            for edge in E:
                u, v, w = edge
                if (c.find(n) == c.find(u) and c.find(n) != c.find(v)) or (
                        c.find(n) == c.find(v) and c.find(n) != c.find(u)):
                    if w < shortest_linked_edge_length:
                        shortest_linked_edge_length = w
                        shortest_linked_edge = edge
            # Avoid selecting the same edge twice.
            if shortest_linked_edge and shortest_linked_edge not in selected_edges:
                selected_edges.add(shortest_linked_edge)
        # According to the selected edges, combine sets.
        for (u, v, w) in selected_edges:
            print(u, v, w)
            c.union(u, v)
            mst_weight += w
        Q.clear()
        for v in V:
            Q.add(c.find(v))
    print("The weight of MST is:", mst_weight)
    
```



## References

1. 维基百科：[Minimum spanning tree](https://en.wikipedia.org/wiki/Minimum_spanning_tree#Related_problems) ；

2. 维基百科：[Borůvka's algorithm](https://en.wikipedia.org/wiki/Bor%C5%AFvka%27s_algorithm)；

3. 《算法导论》（第三版）第 23 章 最小生成树；

4. 《图论引论》（第二版）第 2.3 节 最小生成树;

5. 《图论算法理论实现与应用》第 3.2 节 生成树及最小生成树；

6. 网络文档：[Boruvka’s Algorithm](http://www-student.cse.buffalo.edu/~atri/cse331/fall16/recitations/Recitation10.pdf)；

   

