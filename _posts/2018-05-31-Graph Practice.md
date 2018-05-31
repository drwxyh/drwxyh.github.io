---
layout:     post                    # 使用的布局
title:      笔记：算法练习 			   # 标题 
subtitle:   图论算法理论、实现及应用例题解答（Python）   #副标题
date:       2018-05-31              # 时间
author:     Charles Xu              # 作者
header-img: img/post-bg-waves.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
mathjax: true
tags:                               #标签
	- graph theory

---

## 图论算法理论、实现及应用例题解答（Python）

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

### 例 2.5 红与黑（Red and Black）

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

