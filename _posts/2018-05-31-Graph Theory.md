
---
layout:     post                    # 使用的布局
title:      笔记：算法练习 			   # 标题 
subtitle:   图论算法理论、实现及应用例题解答（python）  #副标题

date:       2018-05-31              # 时间
author:     Charles Xu              # 作者
header-img: img/post-bg-waves.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
mathjax: true
tags:                               #标签
    - graph theory
    - code practice
---
<script type="text/x-mathjax-config"> MathJax.Hub.Config({ tex2jax: {inlineMath: [['$$','$$'],['\\(','\\)']]} }); </script> <script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script>

# 图论算法理论、实现及应用例题解答（Python）

### 例1.1	用邻接矩阵存储有向图，并输出各顶点的出入和入度。

```python

def calculate_in_out():
    data = input('Please type in the number of vertex and edge， separated by space: ').split(' ')
    n = int(data[0])
    m = int(data[1])
    adjacency_matrix = np.zeros((n, n))
    
    i = m
    while i:
        i -= 1
        data = input('Please type in the info of edge: ').split(' ')
        adjacency_matrix[int(data[0]) - 1][int(data[1]) - 1] = 1
        
    in_degree = list()
    out_degree = list()
    for i in range(0, n):
        cnt_in = 0
        cnt_out = 0
        for j in range(0, n):
            cnt_in += adjacency_matrix[j][i]
            cnt_out += adjacency_matrix[i][j]
        in_degree.append(cnt_in)
        out_degree.append(cnt_out)
        
    print(out_degree)
    print(in_degree)
```

###  例1.2  	青蛙的邻居

#### 未名湖附近共有n 个大小湖泊L1, L2, ..., Ln（其中包括未名湖），每个湖泊Li 里住着一只青蛙Fi（1≤i≤n）。如果湖泊Li 和Lj 之间有水路相连，则青蛙Fi 和Fj 互称为邻居。现在已知每只青蛙的邻居数目x1, x2, ..., xn，请你给出每两个湖泊之间的相连关系。

```python
def sort_arr(arr, start):
    temp = list()
    res = list()
    for i in range(start, len(arr)):
        temp.append(arr[i])
    temp = sorted(temp, key=lambda x: x[1], reverse=True)
    for i in range(0, start):
        res.append(arr[i])
    for x in temp:
        res.append(x)
    return res

def is_lake_exist():
    sample_num = int(input('Please type in the number of sample:'))
    while sample_num:
        sample_num -= 1
        lake_num = int(input('Please type in the number of lakes: '))
        data = [int(x) for x in input('Please type in the degree of lake: ').split(' ')]
        lake = list()
        for i in range(0, lake_num):
            lake.append([i, data[i]])

        adjacency_matrix = np.zeros((lake_num, lake_num))
        flag = 1
        for i in range(0, lake_num):
            lake = sort_arr(lake, i)
            # 当前度数最大项的序号为 u, 度数为 d
            u = lake[i][0]
            d = lake[i][1]
            # 最大度数超过剩下顶点数
            if d > lake_num - i - 1:
                flag = 0
                break
            # 其后 d 个顶点的度数减一
            for k in range(1, d + 1):
                v = lake[i + k][0]
                if lake[i + k][1] <= 0:
                    flag = 0
                    break
                lake[i + k][1] -= 1
                adjacency_matrix[u][v] = adjacency_matrix[v][u] = 1

        if flag:
            print('Yes')
            print(adjacency_matrix)
        else:
            print('No')
```

### 例1.3	用邻接表存储有向图，并输出各顶点的入度和出度。

```python
class Vertex:
    def __init__(self, vid):
        self.id = vid
        self.connectedTo = {}  # 使用字典存储相邻节点

    def add_neighbor(self, nbr, weight):
        self.connectedTo[nbr] = weight

    def __str__(self):
        return str(self.id) + 'connectedTo:' + str([x.id for x in self.connectedTo])

    def get_connections(self):
        return self.connectedTo.keys()

    def get_id(self):
        return self.id

    def get_weight(self, nbr):
        return self.connectedTo[nbr]


class Graph:
    def __init__(self):
        self.vertexList = dict()
        self.vertexNum = 0
        self.edgeNum = 0

    def add_vertex(self, vid):
        self.vertexNum += 1
        vertex = Vertex(vid)
        self.vertexList[vid] = vertex

    def get_vertex(self, vid):
        if vid in self.vertexList:
            return self.vertexList[vid]
        else:
            return None

    def add_edge(self, f, t, weight=0):
        self.edgeNum += 1
        if f not in self.vertexList:
            self.add_vertex(f)
        if t not in self.vertexList:
            self.add_vertex(t)
        self.vertexList[f].add_neighbor(self.vertexList[t], weight)

    def get_vertices(self):
        return self.vertexList.values()

    def __iter__(self):
        return iter(self.vertexList.values())


def generate_graph():
    g = Graph()
    n, m = [int(x) for x in input("Please enter the numbers of vertices and edges: ").split(' ')]
    g.vertexNum = n
    g.edgeNum = m

    for x in range(1, n + 1):
        g.add_vertex(x)

    while m:
        m -= 1
        data = [int(x) for x in input("Please enter the info of edge: ").split(' ')]
        if len(data) == 2:
            g.add_edge(data[0], data[1])
        else:
            g.add_edge(data[0], data[1], data[2])

    out_degree = list()
    for x in g.get_vertices():
        out_degree.append(len(x.get_connections()))

    in_degrees = dict((u, 0) for u in g.get_vertices())
    out_degrees = dict((u, 0) for u in g.get_vertices())

    for u in g.get_vertices():
        out_degrees[u] = len(u.get_connections())

    for u in g.get_vertices():
        for v in u.get_connections():
            in_degrees[v] += 1

    print(out_degrees.values())
    print(in_degrees.values())
```

### 例 2.1	骨头的诱惑

#### 一只小狗在一个古老的迷宫里找到一根骨头，当它叼起骨头时，迷宫开始颤抖，它感觉到地面开始下沉。它才明白骨头是一个陷阱，它拼命地试着逃出迷宫。迷宫是一个N*M 大小的长方形，迷宫有一个门。刚开始门是关着的，并且这个门会在第 T 秒钟开启，门只会开启很短的时间（少于一秒），因此小狗必须恰好在第 T 秒达到门的位置。每秒钟，它可以向上、下、左或右移动一步到相邻的方格中。但一旦它移动到相邻的方格，这个方格开始下沉，而且会在下一秒消失。所以，它不能在一个方格中停留超过一秒，也不能回到经过的方格。小狗能成功逃离吗？请你帮助他。

```python
def dfs(cur_x, cur_y, cur_t):
    global escape, door_x, door_y, dir, t
    # 准时到达门产生地点
    if cur_x == door_x and cur_y == door_y and cur_t == t:
        escape = True
        return
    # 剪枝操作
    temp = (t - cur_t) - abs(cur_x - door_x) - abs(cur_y - door_y)
    if temp < 0 or temp % 2:
        return
    # 尝试移动
    for i in range(0, 4):
        # 判断移动是否越界
        if cur_x + dir[i][0] in range(0, n) and cur_y + dir[i][1] in range(0, m):
            if maze[cur_x + dir[i][0]][cur_y + dir[i][1]] != 'X':
                # 将走过的地方设置为墙
                maze[cur_x + dir[i][0]][cur_y + dir[i][1]] = 'X'
                dfs(cur_x + dir[i][0], cur_y + dir[i][1], cur_t + 1)
                if escape:
                    return
                maze[cur_x + dir[i][0]][cur_y + dir[i][1]] = '.'
    return


if __name__ == "__main__":
    maze = []
    dir = [(0, -1), (0, 1), (1, 0), (-1, 0)]
    n, m, t = [int(x) for x in input("Please enter the info of maze:").split(' ')]
    i = n
    while i:
        i -= 1
        maze.append(list(input("Please enter the info of each line:")))

    wall = 0
    for i in range(0, n):
        for j in range(0, m):
            if maze[i][j] == 'D':
                door_x, door_y = i, j
            elif maze[i][j] == 'S':
                start_x, start_y = i, j
            elif maze[i][j] == 'X':
                wall += 1

    if n * m - wall <= t:
        print('No')

    escape = False
    maze[start_x][start_y] = 'X'
    dfs(start_x, start_y, 0)

    if escape:
        print('Yes')
    else:
        print('No')
```

### 例 2.2	油田（Oil deposits）

####GeoSurvComp 地质探测公司负责探测地下油田。每次GeoSurvComp 公司都是在一块长方形的土地上来探测油田。在探测时，他们把这块土地用网格分成若干个小方块，然后逐个分析每块土地，用探测设备探测地下是否有油田。方块土地底下有油田则称为pocket，如果两个pocket相邻，则认为是同一块油田，油田可能覆盖多个pocket。你的工作是计算长方形的土地上有多少个不同的油田。

```python
def find_oil():
    global field, m, n
    for i in range(0, m):
        for j in range(0, n):
            if field[i][j] == 'O':
                return i, j
    return None


def dfs(x, y):
    global field, m, n, dir
    field[x][y] = 'X'
    for i in range(0, 8):
        u = x + dir[i][0]
        v = y + dir[i][1]
        if u < 0 or u >= m or v < 0 or v >= n:
            continue
        if field[u][v] == 'O':
            dfs(u, v)


if __name__ == "__main__":
    field = []
    m, n = [int(x) for x in input("Please enter the info of field(m n):").split(' ')]
    i = m
    while i:
        i -= 1
        field.append(list(input("Please enter the info of each line:")))
    dir = [(-1, -1), (-1, 0), (-1, 1), (0, 1), (1, 1), (1, 0), (1, -1), (0, -1)]

    cnt = 0
    res = find_oil()
    while res:
        x, y = res
        cnt += 1
        dfs(x, y)
        res = find_oil()

    print(cnt)
```

### 例 2.3	农田灌溉（Farm Irrigation ）

####Benny 有一大片农田需要灌溉。农田是一个长方形，被分割成许多小的正方形。每个正方形中都安装了水管。不同的正方形农田中可能安装了不同的水管。一共有11 种水管，分别用字母A～K 标明，如图下图所示。

![](https://ws1.sinaimg.cn/large/006tKfTcgy1fruevegd1jj30l3093weq.jpg)

```PYTHON
def find_non_irrigation():
    global farm, m, n
    for i in range(0, m):
        for j in range(0, n):
            if farm[i][j] != '.':
                return i, j
    return None

def is_connected(a, b, d):
    global flag
    result = 0
    if d == 0:
        if flag[a][1] == '1' and flag[b][3] == '1':
            result = 1
    elif d == 1:
        if flag[a][3] == '1' and flag[b][1] == '1':
            result = 1
    elif d == 2:
        if flag[a][2] == '1' and flag[b][0] == '1':
            result = 1
    else:
        if flag[a][0] == '1' and flag[b][2] == '1':
            result = 1
    return result

def dfs(x, y):
    global farm, m, n, dir
    temp = farm[x][y]
    farm[x][y] = '.'
    print(x, y)
    for i in range(0, 4):
        u = x + dir[i][0]
        v = y + dir[i][1]
        if u < 0 or u >= m or v < 0 or v >= n:
            continue
        if farm[u][v] == '.':
            continue
        if is_connected(temp, farm[u][v], i):
            dfs(u, v)
        else:
            continue

if __name__ == "__main__":
    farm = []
    dir = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    # 按上、右、下、左方向有无水道，将11种管道编码，例如'A':1001,'B':1100……
    flag = {'A': '1001', 'B': '1100', 'C': '0011', 'D': '0110', 'E': '1010', 'F': '0101', 'G': '1101', 'H': '1011',
            'I': '0111', 'J': '1110', 'K': '1111'}
    m, n = [int(x) for x in input("Please enter the info of farm:").split(' ')]
    i = m

    while i:
        i -= 1
        farm.append(list(input("Please enter the info of each line:")))

    cnt = 0
    res = find_non_irrigation()
    while res:
        x, y = res
        dfs(x, y)
        cnt += 1
        res = find_non_irrigation()

    print(cnt)
```

### 例 2.4	Gnome Tetravex 游戏 

####哈特近来一直在玩有趣的Gnome Tetravex 游戏。在游戏开始时，玩家会得 n×n（n≤5）个正方形。每个正方形都被分成4 个标有数字的三角形（数字的范围是0 到9）。这四个三角形分别被称为“左三角形”、“右三角形”、“上三角形”和“下三角形”。例如，图2.12(a)是2×2 的正方形的一个初始状态。

![](https://ws4.sinaimg.cn/large/006tKfTcgy1fruh2nouwqj30ft076q35.jpg)

####玩家需要重排正方形，到达目标状态。在目标状态中，任何两个相邻正方形的相邻三角形上的数字都相同。图2.12(b)是一个目标状态的例子。看起来这个游戏并不难。但是说实话，哈特并不擅长这种游戏，他能成功地完成最简单的游戏，但是当他面对一个更复杂的游戏时，他根本无法找到解法。某一天，当哈特玩一个非常复杂的游戏的时候，他大喊到：“电脑在耍我！不可能解出这个游戏。”对于这样可怜的玩家，帮助他的最好方法是告诉他游戏是否有解。如果他知道游戏是无解的，他就不需要再把如此多的时间浪费在它上面了。

```python
from collections import namedtuple, OrderedDict

def dfs(cnt):
    global board, n, m, square_dict, kinds
    if cnt == n * n:
        return True

    for i in range(0, m):
        item = kinds[i]
        # 此类方块已用完
        if square_dict[item] == 0:
            continue
        # 非第一列，不匹配左边方块
        if cnt > 0:
            if cnt % n and item.left != kinds[board[cnt - 1]].right:
                continue

        # 非第一行，不匹配上方方块
        if cnt > n:
            if cnt // n and item.up != kinds[board[cnt - n]].down:
                continue

        square_dict[item] -= 1
        board[cnt] = i
        if dfs(cnt + 1):
            return True
        else:
            square_dict[item] += 1

    return False

if __name__ == "__main__":
    n = int(input("Please enter the size of the big square:"))
    i = n * n
    Square = namedtuple('Square', ['up', 'right', 'down', 'left'])
    squares = list()  # 存放方块数据
    board = [0] * (n * n)  # 记录每个位置放置方块的种类
    square_dict = OrderedDict()  # 记录每种方块的数量

    while i:
        i -= 1
        data = [int(x) for x in input("Please enter the value of each square:").split(' ')]
        squares.append(Square(*data))

    for square in squares:
        if square not in square_dict:
            square_dict[square] = 1
        else:
            square_dict[square] += 1
    kinds = list()

    for item in square_dict.keys():
        kinds.append(item)

    m = len(kinds)

    if dfs(0):
        print("Possible.")
    else:
        print("Impossible.")
```

### 例 2.5	红与黑（Red and Black）

####有一个长方形的房间，房间里的地面上布满了正方形的瓷砖，瓷砖要么是红色，要么是黑色。一男子站在其中一块黑色的瓷砖上。男子可以向他四周的瓷砖上移动，但不能移动到红色的瓷砖上，只能在黑色的瓷砖上移动。本题的目的就是要编写程序，计算他在这个房间里可以到达的黑色瓷砖的数量。

```python
def dfs(x, y):
    global ground, dir, width, height, cnt
    cnt += 1
    ground[x][y] = 'M'
    for i in range(0, 4):
        u = x + dir[i][0]
        v = y + dir[i][1]
        if u in range(0, width) and v in range(0, height):
            # 当且仅当移动到的格子为黑格时，继续搜索
            if ground[u][v] == 'B':
                dfs(u, v)

if __name__ == "__main__":
    ground = list()
    dir = [(1, 0), (0, 1), (0, -1), (-1, 0)]
    cnt = 0
    print("Please enter the width and height:")
    height, width = [int(x) for x in input().split(' ')]
    i = width
    print("Please enter the info of each line(Red:R,Black:B,Man:M):")
    while i:
        i -= 1
        ground.append(list(input()))

    for i in range(0, width):
        for j in range(0, height):
            if ground[i][j] == 'M':
                u, v = i, j

    dfs(u, v)
    print(cnt)
```

### 例 2.6	营救(Rescue) 

#### Angel 被MOLIGPY 抓住了，她被关在监狱里。监狱可以用一个N×M 的矩阵来描述，1<N, M ≤ 200。监狱由N×M 个方格组成，每个方格中可能为墙壁、道路、警卫、Angel 或Angel 的朋友。Angel 的朋友想去营救Angel。他的任务是接近Angel。约定“接近Angel”的意思是到达Angel被关的位置。如果Angel 的朋友想到达某个方格，但方格中有警卫，那么必须杀死警卫，才能到达这个方格。假定Angel 的朋友向上、下、左、右移动一步用时为1 个单位时间，杀死警卫用时也为1 个单位时间。假定Angel 的朋友是如此强壮，可以杀死所有的警卫。你的任务是计算Angel 的朋友接近Angel 至少需要多长时间，只能向上、下、左、右移动，而且墙壁不能通过。

```python
from collections import deque, namedtuple
import numpy as np
import sys

def bfs(p):
    global q, cell, directions, min_time, a_y, a_y
    # 加入队列
    q.append(p)
    while q:
        hd = q.pop()
        for i in range(0, 4):
            u = hd.x + directions[i][0]
            v = hd.y + directions[i][1]
            if u in range(0, n) and v in range(0, m) and cell[u][v] != '#':
                if cell[u][v] == 'x':
                    t = Point(u, v, hd.step + 1, hd.time + 2)
                else:
                    t = Point(u, v, hd.step + 1, hd.time + 1)
                if t.time < min_time[u][v]:
                    q.append(t)
                    min_time[u][v] = t.time
    return min_time[a_x][a_y]

if __name__ == "__main__":
    cell = []
    Point = namedtuple('Point', ['x', 'y', 'step', 'time'])
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    with open('2.7') as fp:
        n, m = [int(x) for x in fp.readline().split(' ')]
        i = n
        while i:
            i -= 1
            cell.append(list(fp.readline().strip('\n')))

    q = deque()
    min_time = np.zeros((n, m), dtype=int)  # 存储从救援人位置到达i,j的最短时间
    for i in range(0, n):
        for j in range(0, m):
            min_time[i][j] = sys.maxsize
            if cell[i][j] == 'a':
                a_x, a_y = i, j
            if cell[i][j] == 'r':
                r_x, r_y = i, j
    s = Point(r_x, r_y, 0, 0)
    min_time[r_x][r_y] = 0
    res = bfs(s)

    if res == sys.maxsize:
        print("Poor angel has to stay in the cell all his life.")
    else:
        print("The least time cost: %d" % res)
```

### 例 2.7	公共汽车通票（bus pass）

#### 你乘坐公共汽车途经一些地区，旅程费用为每张车票费用的总和。因此，你想知道如果买通票的话是否要优惠一些。你的国家的公交系统按如下方式运转：当你买一张公共汽车通票时，你必须指定一个中心地区，以及一个星形阈值；当你持通票在某个地区乘坐公共汽车时，只要该地区离中心地区的距离（并不是路程的距离，而是路程上地区数目）小于星形阈值，则可以免费乘坐。例如，如果你选择的星形阈值为1，则你只能在中心地区免费乘坐公交汽车；如果你选择的星形阈值为2，则在中心地区及与中心地区相邻的地区里都可以免费乘坐公交汽车；等等。你列出了你经常要乘坐的公交线路，你希望能确定一个最小的星形阈值，使得所有的公交线路在通票范围内都是免费的。但这并不是一件容易的工作。例如，如图2.16 所示的地区分布图：你想乘坐公交车从A 到B，以及从B 到D。则最好的中心地区编号为7400，这样你需要选择的阈值为最小值4。注意，在你的公交线路中并不需要途经你选择的中心地区。 

![](http://oz9e7ei0y.bkt.clouddn.com/2018-06-01-075827.jpg)

```python
from collections import deque


def bfs(s):
    dist = 1
    Q = list()
    Q.append(deque())
    Q.append(deque())
    a, b = 0, 1
    if check_by[s] < cur:
        Q[b].append(s)
        check_by[s] = cur
        gap_num[s] = max(gap_num[s], dist)

    while Q[b]:
        a, b = b, a
        while Q[a]:
            cur_id = Q[a].pop()
            for i in range(0, neighbor_num[str(cur_id + 7400)]):
                next_id = neighbor_area[str(cur_id + 7400)][i]
                if check_by[next_id] < cur:
                    Q[b].append(next_id)
                    check_by[next_id] = cur
                    gap_num[next_id] = max(gap_num[next_id], dist + 1)
        dist += 1

if __name__ == "__main__":
    stations = list()  # 存储公交车路线上的站点
    neighbor_num = dict()  # 存储相邻区域数目
    neighbor_area = dict()  # 存储相邻区域编号
    cur = 0
    with open('2.8') as fp:
        nz, nr = [int(x) for x in fp.readline().strip('\n').split(' ')]
        check_by = [-1 for x in range(0, nz)]  
        gap_num = [0 for x in range(0, nz)]  
        i = nz
        while i:
            i -= 1
            data = fp.readline().strip('\n').split(' ')
            if data[0] not in neighbor_num:
                neighbor_num[data[0]] = int(data[1])
            if data[0] not in neighbor_area:
                neighbor_area[data[0]] = list()
                for j in range(2, len(data)):
                    neighbor_area[data[0]].append(int(data[j]) - 7400)

        j = nr
        while j:
            j -= 1
            data = [int(x) - 7400 for x in fp.readline().strip('\n').split(' ') if int(x) > 7000]
            for item in data:
                if item not in stations:
                    stations.append(item)

    for i in stations:
        bfs(i)
        cur += 1

    t = gap_num.index(min(gap_num))
    center = str(7400 + t)
    gap = gap_num[t]
    print('%d %s' % (gap, center))
```

简要思路：题目的目的是要选定一个中心区，使得其与线路上各区域的距离的最大值最小化。一种比较简单的思路是，分别以每一个区域为中心区，进行广度优先搜索，然后得到其与线路上各区域距离的最大值；比较每个区域得到的最大值，用于最小最大值的那个区域即为所求中心区，对应的最大值为半径大小。这种思路显然可以求出正确的解，但是这样的话每一个区域都要进行一次深度优先搜索，计算量比较大。我们可以反过来想，中心区到路线上各个区域的距离的最大值最小，也就是路线上各点到中心区的距离的最大值最小，这样我们可以以路线上各点作为BFS的起点，进行 n (路线经过的区域数)次BFS。BFS类似水波的扩散，起点区域的层次为1，与其接壤的区域层次为2，我们使用a, b两个队列，a存储当前搜索层，b存储当前层的下一层。cur作为索引标记，用于检测在第cur次BFS搜索中有没有被检测过。没有检测过就要检测，检测的主要内容是，将该区域加入下一层，打上本轮检测标记，更新路线上区域到其距离的最大值。一层中所有区域检测完毕，层次深度加一，对下一层区域进行检测，直到不存在下一层，退出搜索。

### 例 2.8	蛇和梯子游戏(Snakes & Ladders) 

#### 蛇和梯子游戏是一个非常流行的游戏。游戏采用 N×N 的棋盘，方格编号从 1 到 N^2，如图(a)所示。棋盘中分布着蛇和梯子。玩家 A 和 B 的起始位置都在方格1 处，每个玩家只能顺着如图(b)所示的方向走棋；每次走棋之前，先投骰子，得到一个点数，然后走该点数步。

![](http://oz9e7ei0y.bkt.clouddn.com/2018-06-02-100152.jpg)

#### 惩罚：如果玩家的落脚地刚好是在蛇头，则降至蛇尾所在方格处；

#### 奖励：如果玩家落脚地刚好是在梯子底部，则顺着梯子爬到顶部所在方格处。

#### 最先到达 N^2 那一方获胜。

#### 关于棋盘分布有两点值得注意：

#### 1) 第1 个和最后一个方格处没有梯子和蛇

#### 2) 并且蛇和梯子不能相邻，也就是在任何放置两者首尾的方格之间至少还有一个未放置任何东西的格子。

#### 你的朋友 Fadi 希望你编写一个程序，帮助他赢得比赛。Fadi 是一个职业骗子，他投骰子可以得到任何期望的点数（1 至 6）。Fadi 希望知道他至少需要投多少次骰子才能赢得比赛。例如，在图(a)所示的棋盘布局图中，Fadi 投三次骰子就可以赢得比赛：首先投骰子得到点数 4，到达方格 5 的位置，顺着梯子爬到方格16 的位置；然后投骰子得到点数 4，到达方格 20 的位置，然后顺着梯子爬到方格 33 的位置，接下来只要投骰子得到点数 3 就可以赢得比赛了。

```python
from collections import namedtuple

if __name__ == "__main__":
    Snake = namedtuple('Snake', ['f', 't']) # type: __main__.Snake
    Lift = namedtuple('Lift', ['f', 't']) # type: __main__.Lift
    with open('2.9') as fp:
        n, s, l = [int(x) for x in fp.readline().strip('\n').split(' ')]
        N = n * n
        graph = ['O' for i in range(0, N)]

        snake_data = [int(x) for x in fp.readline().strip('\n').split(' ')]
        snake_list = list()
        for i in range(0, s):
            graph[snake_data[2 * i] - 1] = Snake(snake_data[2 * i] - 1, snake_data[2 * i + 1] - 1)

        lift_data = [int(x) for x in fp.readline().strip('\n').split(' ')]
        lift_list = list()
        for i in range(0, l):
            graph[lift_data[2 * i] - 1] = Lift(lift_data[2 * i] - 1, lift_data[2 * i + 1] - 1)

        graph[0] = 'V'
        step = 0
        while graph[N-1] != 'V':
            step += 1
            search_list = list()
            for i in range(0, N):
                if graph[i] == 'V':
                    search_list.append(i)

            for x in search_list:
                for j in range(1, 7):
                    u = x + j
                    if u in range(0, N):
                        if graph[u] == 'O' or graph[u] == 'V':
                            graph[u] = 'V'
                        else:
                            graph[graph[u].t] = 'V'

        print(step)
```

### 例 2.9	倍数(Multiple) 

#### 编写程序，实现：给定一个自然数N，N 的范围为[0, 4999]，以及M 个不同的十进制数字X1,X2, …, XM（至少一个，即M≥1），求N 的最小的正整数倍数，满足：N 的每位数字均为X1, X2, …,XM 中的一个。

```python
from collections import deque, namedtuple
import numpy as np

def bfs():
    global n, m, data
    Node = namedtuple('Node', ['v', 'c'])
    visited = np.zeros(n, dtype=int)
    Q = deque()
    Q.append(Node(0, 0))
    while Q:
        t = Q.popleft()
        for i in range(0, m):
            k = t.c * 10 + data[i]
            if not k:
                continue
            k = k % n
            if k == 0:
                return t.v * 10 + data[i]
            if not visited[k]:
                visited[k] = 1
                Q.append(Node(t.v * 10 + data[i], k))
    return 0

if __name__ == "__main__":
    with open('2.9') as fp:
        n = int(fp.readline())
        m = int(fp.readline())
        data = sorted([int(x) for x in fp.readlines()])
    res = bfs()
    print(res)
```

简单思路：决定一个数能否被 n 整除的决定因素在余数 C，对于除 n 拥有相同余数的数，我们只需要记录其中最小的那一个，队列中存储的是数值和余数对，我们将余数乘以10然后加上集合中的数字 i ，检查能否被 n 整除，如果能整除，那么我们想要的数就是原数*10+i，如果产生了一个新的余数，那么我们首先将这个余数标记，表示这个余数已经被检查了，然后将新数和余数加入队列。

### 练习 2.5~2.8 以后做

### 例 2.10	对输入的有向图进行拓扑排序，并输出一个拓扑有序序列；如果存在有向环，则给出提示信息。

```python
class ArcNode:
    def __init__(self, to, nx=None):
        self.to = to
        self.next = nx


def topo_sort():
    global count, vertex_list
    res = list()
    top = -1
    has_circle = False

    for i in range(0, n):
        if count[i] == 0:
            count[i] = top
            top = i

    for i in range(0, n):
        if top == -1:
            has_circle = True
            break
        else:
            j = top
            top = count[top]
            res.append(j + 1)
            t = vertex_list[j]
            while t:
                k = t.to
                count[k] -= 1
                if count[k] == 0:
                    count[k] = top
                    top = k
                t = t.next

    if has_circle:
        print('Network has a cycle!')
    else:
        print(res)


if __name__ == "__main__":
    with open('2.10') as fp:
        n, m = [int(x) for x in fp.readline().strip('\n').split(' ')]
        data = list()
        count = [0 for i in range(0, n)]
        vertex_list = [0 for i in range(0, n)]
        for x in fp.readlines():
            data.append(tuple(int(i) - 1 for i in x.strip('\n').split(' ')))

    for d in data:
        t = ArcNode(d[1])
        count[d[1]] += 1
        if vertex_list[d[0]] == 0:
            vertex_list[d[0]] = t
        else:
            p = vertex_list[d[0]]
            while p.next:
                p = p.next
            p.next = t

    topo_sort()
```

### 例 2.11	将元素排序（Sorting It All Out）

#### 由一些不同元素组成的升序序列是可以用若干个小于号将所有的元素按从最小到最大的顺序排列起来的序列。例如，排序后的序列为A, B, C, D，这意味着A < B、B < C 和C < D。在本题中，给定一组形如A < B 的关系式，你的任务是判定是否存在一个有序序列。

```python
class ArcNode:
    def __init__(self, to, next=None):
        self.to = to
        self.next = next

def topo_sort(c):
    res = ''
    has_circle = False
    step = 0
    top = -1
    for i in range(0, n):
        if count[i] == 0:
            count[i] = top
            top = i

    for i in range(0, c):
        step = i + 1
        if top == -1:
            has_circle = True
            break
        else:
            j = top
            res += chr(j + 65)
            top = count[j]
            p = char[j]
            while p:
                k = p.to
                count[k] -= 1
                if count[k] == 0:
                    count[k] = top
                    top = k
                p = p.next

    if not has_circle and len(res) == n:
        print('Sorted sequence determined after %d relations: %s.' % (step, res))
    else:
        if has_circle:
            print('Inconsistency found after %d relations.' % step)
        else:
            print('Sorted sequence cannot be determined.')

if __name__ == "__main__":
    with open('2.11') as fp:
        n, m = [int(x) for x in fp.readline().strip('\n').split(' ')]
        char = [0 for i in range(0, n)]
        count = [0 for i in range(0, n)]
        c = 0
        for line in fp.readlines():
            u, v = line.strip('\n').split('<')
            c += 1
            x = ord(u) - 65
            y = ord(v) - 65
            t = ArcNode(y)
            count[y] += 1
            if char[x] == 0:
                char[x] = t
            else:
                p = char[x]
                while p.next:
                    p = p.next
                p.next = t

    topo_sort(c)
```

