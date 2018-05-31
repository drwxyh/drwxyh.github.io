---
layout:     post                    # 使用的布局（不需要改）
title:      笔记：算法复习 			   # 标题 
subtitle:   重新学习算法的一些笔记	   # 副标题
date:       2018-04-27              # 时间
author:     Charles Xu              # 作者
header-img: img/post-bg-space-0.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
mathjax: true
tags:                               #标签
    - book note
    - algorithm
---
<script type="text/x-mathjax-config"> MathJax.Hub.Config({ tex2jax: {inlineMath: [['$$','$$'],['\\(','\\)']]} }); </script> <script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script>
## 题外话
真正接触算法是在大二，刚开始的时候兴趣盎然，还常常早起去抢第一排的座位听课，不过渐渐就对老师所讲的内容失去了兴趣，所以最终算法也是学的一塌糊涂。倒不是说老师讲课不认真，老先生讲课还是很投入的，无奈身体不好，声音小，记忆力衰退（每节课上课都要问一遍我们是什么专业），对于刚接触的我们来讲算法本就艰深难懂，而老师的授课技巧又比较单调，所以很多同学都失去了学习兴趣，上课就不听了。最终，老先生也对我们很失望，开始每节课对我们进嘲讽（涉及对另一学院同学的歧视，此处不谈）。后来，听说老先生因为身体虚弱，在校园里走路时晕倒被送去医院，心里也是一阵愧疚，一阵心疼。老先生的性格适合做研究，不适合教书，所以人还是应该扬长避短，不合适的事做得再努力，也只会弄巧成拙。

研究生阶段，再次学习算法，讲课的倪老师的讲课讲技巧很好，常常拿了解到的面试问题给我们举例子，提高我们的学习兴趣。由于同学们的基础不一样，所以讲课的内容比较基础，整体上属于本科式的教学模式，算是把本科不实的算法基础又夯实了一遍。之后因为加入吴老师的实验室，所以去听他的课程，才发现原来算法有另一种讲法——启发式。不管是博弈论还是近似算法，吴老师都是从最简单的例子开始，给同学们提问题，在大家有了自己的思考之后，引导大家给出靠谱的答案，然后又抛出更深层次的问题，这样一步一步走下来，在学习知识的同时，也有了自己的思考，对于算法的理解也更为透彻。我认为吴老师的授课方式才是真正适合研究生的授课方式。

当然，师傅领进门，修行在个人。所有辛勤工作的老师都是值得尊敬的，算法没学好最大的问题还是在于我自己没有深入钻研。亡羊补牢，未为晚也，从现在开始重新学习吧。
## 算法（第4版）
### 基础编程模型
- 原始数据类型：整型(int)、浮点型(double)、布尔型(boolean)、字符型(char)、long、short、byte、float；

- 语句：声明、赋值、条件、循环、调用和返回(和静态方法有关)；

- 创建数组的三步：1、生命数组的名字和类型；2、创建数组；3、初始化数组元素；

#### API
- Math函数库API：
	- abs：绝对值
	- max：两数较大值
	- min：两数较小值
	- sin：正弦
	- cos：余弦
	- tan：正切
	- exp：指数函数 $$e^a$$
	- log：求对数
	- pow：求 $$a^b$$
	- random：无参数，返回[0,1)间随机数
	- sqrt：求平方根
	- E：常数 e
	- PI：常数 π

- 随机数静态方法库API：
	- random：0~1之间实数
	- uniform：根据参数个数和类型而异，可返回区间内的整数或实数
	- bernoulli：伯努利，返回真的概率为p
	- gaussian：高斯分布，默认期望值为0，标准差为1
	- discreate：返回 i 的概率为 a[i]
	- shuffle：将数组 a 随机排序

- 数据分析静态数据库API：
	- max：最大值
	- min：最小值
	- mean：平均值
	- var：采样方差
	- stddev：采样标准差
	- median：中位数

- 字符串：
	- String和数字的转换：parseInt(), parseDouble(), toString()

- 标准输出StdOut：
	- print：将参数放到标准输出；
	- println：附加换行符；
	- printf：格式化输出

- 读取和写入数组的静态方法API：
	- class In
		- readInts：读取多个 int 值；
		- readDoubles：读取多个 double 值；
		- readStrings：读取多个 String 值；
	- class Out
		- write：根据参数的类型，写入不同类型的值；

- 标准绘图库StdDraw的静态方法API：
	- point：绘制点
	- line：绘制线
	- text：绘制文本
	- circle：画圆
	- filledCircle：实心圆
	- ellipse：椭圆
	- filledellipse：实心椭圆
	- square：正方形
	- filledsquare：实心正方形
	- rectangle：矩形
	- filledrectangle：实心矩形
	- setXscale：设置x的范围
	- setYscale：设置y的范围
	- setPenRadius：设置画笔粗细半径
	- setPenColor：设置画笔颜色
	- setFont：设置文本字体
	- setCanvasSize：设置画布的高宽
	- clear：清空画布，并用颜色C填充
	- show：显示所有图形，并暂停t秒

### 数据的抽象
抽象数据类型（ADT）：是一种能对使用者隐藏数据表示的数据类型。实现数据和函数的关联，并将数据的表示方式隐藏起来。

#### 实现
- 第一部分语句定义表示数据类型的值的实例变量；
- 实现对数据类型的值得操作的构造函数和实例方法；
![](https://ws4.sinaimg.cn/large/006tNc79gy1fqt7m5d0fwj30g20h4jvc.jpg)

- 实例变量：
	- 要定义数据类型的值；
	- 局部变量每一时刻对应一个值，而实例变量可以对应无数值；
	- 为了隐藏数据，通常修饰符为 private，如果值在初始化后不允许被改变，使用 final；

- 构造函数：
	- 不定义，有默认的；
	- 与类名相同，无返回值；

- 等价性：
	- a == b：判断两者引用是否相同；
	- x.equals(y)：判断数据的值是否相同，拥有自反性、对称性、传递性；

- 断言：是一条需要在程序某处确认为 true 的布尔表达式，如果表达式为false，程序将终止并报告一条出错信息；’assert index >= 0 : "Negative index in method x";‘

答疑：

- 为什么要区别原始数据类型和引用类型？因为性能原因，原始数据类型更接近计算机硬件所支持的数据类型，使用原始类型程序运行得更快；

- 创建对象，不用 new 会如何？这样的形式好像在调用一个静态方法，会返回找不到函数错误；

- Java如何实现垃圾回收？
	- 一种方式是使用指针（机器地址），访问数据的速度更快；
	- 一种方式是使用句柄（指针的指针），更好地实现垃圾回收；

- 什么是 null？不指向任何对象的字面量；

- 什么是弃用（deprecate）的方法？不再被支持但是为了保持兼容性而留在API中的方法；

### 背包、对象和列
![](https://ws2.sinaimg.cn/large/006tNc79gy1fqt98i7s4wj30k30bytbg.jpg)
![](https://ws1.sinaimg.cn/large/006tNc79gy1fqt9927tv9j30jv05vgn6.jpg)

- 背包：不支持从中删除元素的几何数据类型，它的目的就是帮助用例手机元素并迭代遍历所有收集到的元素；
![](https://ws3.sinaimg.cn/large/006tNc79gy1fqtmcg6ngfj31e00roq42.jpg)
- FIFO队列：元素的处理顺序就是它们被添加到队列中的顺序；
![](https://ws4.sinaimg.cn/large/006tNc79gy1fqtm1hx7zsj31e0184mz0.jpg)
- 下压栈：是一种后进先出的集合类型，使用foreach遍历时，元素的处理顺序和被压入的顺序正好相反；
动态调整数组大小的实现
	- 基于动态数组：
![](https://ws1.sinaimg.cn/large/006tNc79gy1fqtdmqwyyoj31e017qjtc.jpg)
	- 基于链表
![](https://ws1.sinaimg.cn/large/006tNc79gy1fqtli56a1pj31dw12ign7.jpg)
- 定容栈：
![](https://ws2.sinaimg.cn/large/006tNc79gy1fqtcg75046j31e00l0q3n.jpg)

- 链表：一种递归的数据结构，或者为空，或者指向下一个节点（node）的引用，该节点包含一个泛型变量和一个指向另一条链表的引用；

#### 泛型
概念：集合类抽象数据类型可以用来存储任意类型的数据；

迭代器：是一个实现了`hasNext()`和`next()`方法的类的对象；

```
public interface Iterator<Item>
{	
	boolean hasNext();
	Item next();
	void remove();
}
```
可迭代的集合类型：可以通过迭代访问集合中的所有元素；

- 实现：
	- 集合数据类型必须实现一个 `iterator()` 方法并返回一个 Iterator 对象，
		- 一个类可迭代，第一步就是在声明中加入 `implements Iterable<Item>`；
		- 然后，在类中添加一个方法 `iterator()` 并返回一个迭代器 `Iterator`
	- Iterator 类必须包含两个方法：`hasNext()`，返回一个布尔值，`next()`，返回集合中的一个泛型元素；
	
答疑：

- 泛型的替代方案：为每种类型的数据都实现一个不同的集合数据类型。
- 如何创建一个字符串栈的数组？使用类型转换`Stack<String>[] a = (Stack<String[]) new Stack[N])`	
- 栈为空时，pop会发生什么？具体取决于实现，可能会返回空指针异常；
- 为什么要将 Node 实现为嵌套私有类？嵌套类的实例变量只有包含它的类可以直接访问，无须再声明为 private；
- 输入`javac Stack.class`时会出现两个文件：`Stack.class`和`Stack$Node.class`，其中第二个文件是内部类`Node`创建的，Java用 $ 分隔外部类和内部类；
- 可以使用 foreach 循环访问数组，尽管没有实现Iterable接口；不能使用 foreach 循环访问字符串，因为没有实现Iterable接口；

### 算法分析

#### 计时器
![](https://ws1.sinaimg.cn/large/006tNc79gy1fqtmyrpz91j30k00j1agl.jpg)

#### 数学模型
建立数学模型的步骤：

- 确定输入模型，定义问题的规模；
- 识别内循环；
- 根据内循环中的操作确定成本模型；
- 对于给定的输入，判断这些操作的执行频率；

#### 常见函数
![](https://ws3.sinaimg.cn/large/006tNc79gy1fqtnhja674j30jy0dpmze.jpg)

#### 增长级数分类
![](https://ws2.sinaimg.cn/large/006tNc79gy1fqtnivvlpij30jy0bht9d.jpg)

#### 倍率实验
判断程序实验大致增长数量级的实验。
![](https://ws4.sinaimg.cn/large/006tNc79gy1fqtnnrfq8zj30jn03rmyk.jpg)

## 排序
学习排序算法的三大意义：

- 对排序算法的分析将有助于全面理解比较算法性能的方法；
- 类似的技术也能有效解决其他类型问题；
- 排序是很多问题的第一步；

### 初级排序算法

#### 选择排序
思想：

- 首先，找到最小的元素，和数组第一个元素交换；
- 其次，找到第二小的元素，和数组第二个元素交换；
- 如此往复，直到整个数组排序完成；

分析：
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fqshl09h5ij30wy08qjsm.jpg)

特点：

- 运行时间和输入无关；
- 数据移动是最少的，每次交换都会改变两个数组元素的值，交换次数和数组大小是线性关系；

代码：
![](https://ws3.sinaimg.cn/large/006tNc79gy1fqtv354eltj31dw0qu75e.jpg)

#### 插入排序
思想：不断将新的元素插入到已经有序的元素序列中去。

分析：
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fqsilypqk8j30wy0do40i.jpg)
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fqsiut832jj30wx07mwfe.jpg)
特点：

- 对于某些常见类型的非随机数组很有效；
- 对于已经有序的数组，能够发现每个元素都已经在合适位置上，运行时间为线性；

代码：
![](https://ws4.sinaimg.cn/large/006tKfTcgy1fqsislhz84j31e00ayt8x.jpg)

#### 两种排序算法的比较
分析：
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fqsiy71qnuj30wy0b80uf.jpg)

代码：
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fqud0qocsvj31e0104dhe.jpg)

### 希尔排序
思想：是使数组中任意间隔为 h 的元素都是有序的。
	- 用插入排序将h个子数组独立的排序；
	- 逐渐的减小 h 进行子数组排序，最终 h = 1；

图例：
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fqsmc2v8p5j30n007uaa0.jpg)

特征：

- 一般情况下，希尔排序比插入排序和选择排序要快得多，并且数组越大，优势越大；
- 在最坏情况下，运行时间不到平方级，比较次数和 $$N^{3/2}$$ 成正比；

分析：
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fqsmk8ztcpj30wy07nq3u.jpg)

代码：
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fqsnonobu4j31dy0gujrs.jpg)

### 归并排序
思想：递归地将数组分成凉拌分别排序，然后将排好序的数组归并成一个有序的大数组；

特征：

- 任意长度为 N 的数组排序所需时间和 NlgN 成正比；
- 苏旭额外空间和 N 成正比；



#### 自顶向下的归并排序
原地归并：

- 先将数组拷贝到一个辅助数组中；
- 然后进行条件判断，排序：
	- 左边元素取完，取右边的；
	- 右边元素取完，取左边的；
	- 左边元素小于右边元素，存左边的；
	- 左边元素不小于右边元素，存右边的；

代码：
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fquf51vqz6j31e00ya75u.jpg)

分析：
![](https://ws4.sinaimg.cn/large/006tKfTcgy1fqufa8pzsdj30ji0i5juz.jpg)

#### 自底向上的归并排序
归并程序：多次遍历整个数组，根据子数组的大小进行两两归并；

代码：
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fqugpx4ci8j31e20t80u2.jpg)

分析：
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fqugqxt9lvj30jn03naam.jpg)

答疑：

- 归并和希尔排序的运行时间差距在常数级别之内；
- 归并的辅助数组尽量不要写成merge的局部变量，不然每次合并都要创建数组，效率低下；比较好的方案是将辅助数组变为sort方法的局部变量，并将其作为参数传给merge；

### 快速排序
思想：分治思想的典型应用，将整个数组递归地分成两个子数组排序，当子数组有序时，整个数组也有序；

特征：

- 原地排序，辅助栈小；
- 对大小为 N 的数组排序，所需时间与 NlgN 成正比；

代码：
![](https://ws2.sinaimg.cn/large/006tKfTcgy1fquho6mw7zj31dy0u6gmx.jpg)

分析：
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fquhq042osj30jk0djn3b.jpg)
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fquhpqvdegj30jf08o78a.jpg)

改进：

- 切换到插入排序：插入排序在数组长度较短时表现优于快速排序；

```
if(hi <= lo)
	return；
// 替换为：
if(hi <= lo + M){
	Insertion.Sort(a, lo, hi);
	return;
}
```

- 三取样法：使用子数组一部分元素的中位数作为主元来切分数组，根据经验将取样大小设为3并用大小居中的元素切分效果最好；

- 熵最优排序：避免对元素相同数组进行划分排序，简单的想法是将数组划分为三个部分，分别对应小于、等于、大于切分元素的数组元素；

### 优先队列
思想：不要求数组整体有序，每次处理当前元素的最大值，然后加入新的元素，如此循环下去；

API设计：
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fquimdwfyaj30jw038t9p.jpg)
![](https://ws4.sinaimg.cn/large/006tKfTcgy1fquinhfpdzj30k104ygmy.jpg)

实现：

- 无序数组：基于下压栈，insert 和 push 保持不变，添加一段类似选择排序内循环的代码，将最大元素和栈顶元素交换，然后删除；
- 有序数组：修改 insert，每次插入时保证数组整体有序，这样删除最大元素的操作就和 pop 保持一致；
- 基于链表：修改 pop，查询链表找到并返回最大元素，然后删除该元素；
- 基于大顶堆：每次删除堆顶元素；

### 堆排序
概念：

- 堆有序：一棵二叉树的每个结点都大于等于它的两个子节点；
- 二叉堆：一组能够用对有序的完全二叉树排序的元素，并在数组中按照层级存储（不使用第一个位置）。

实现：
- 堆的构造，将原始数据构造成一个堆；
- 按递减顺序取出所有元素并得到排序结果；

代码：
![](https://ws2.sinaimg.cn/large/006tKfTcgy1fquriq698ej31e010stat.jpg)

答疑：

- 堆为什么不使用a[0]，技术上实现并不困难，这样可以简化计算，而且某些应用中会把a[0]作为a[1]的父节点；

### 总结
稳定性：相同元素的相对位置在排序后不变；

- 稳定排序算法：插入排序，归并排序；
- 不稳定排序算法：选择排序，希尔排序，堆排序，快速排序；

特点：
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fqurw6pe2uj30k008qtbl.jpg)

实际使用：
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fqus08ri37j30jm052ac9.jpg)

实际问题：

- 统计数组中不重复元素个数；
- 中位数与顺序统计（寻找第 k 小元素，使用快排可以在线性时间找出）；
- Prim， Kruskal，Dijkstra算法；
- 霍夫曼压缩算法；


Java系统库的排序方法：

- 每种原始数据类型都有一个不同的排序方法；
- 一个适用于多有实现了Comparable接口的数据类型的方法；
- 一个适用于实现了比较器Comparator的数据类型的排序方法；
- ……

Java系统程序猿对原始数据类型使用（三向切分）的快速排序，对引用类型使用归并排序。

## 查找
### 符号表
定义：符号表是一种存储键值对的数据结构，支持两种操作：插入（put），即将一组新的键值对存入表中；查找（get），即根据给定的键得到相应的值；
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fqsnuqj07vj30x409dgm4.jpg)
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fqsny99v77j30x70by0tg.jpg)
![](https://ws4.sinaimg.cn/large/006tKfTcgy1fqso1i3vuej30x90gymy8.jpg)
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fqso4d7k7qj30xc06yt8z.jpg)
用例：
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fqsodppomxj30ho0bq74b.jpg)
成本模型：
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fqso6spa1qj30wy03d74l.jpg)
分析：
![](https://ws4.sinaimg.cn/large/006tKfTcgy1fqsoq69x7mj30wy07nq3u.jpg)

### 无序链表中的顺序查找
代码：
![](https://ws4.sinaimg.cn/large/006tKfTcgy1fqsonwhzrbj31kw0vjgni.jpg)
### 二分查找基于有序数组
代码：
![](https://ws2.sinaimg.cn/large/006tKfTcgy1fqsp56bd4fj31kw140abw.jpg)
分析：
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fqspaucy4yj30wv0d975y.jpg)
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fqspboe9fkj30wy0cct9b.jpg)
总结：
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fqv0e2jvipj30vd07gdgh.jpg)
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fqv2k9svf9j30vb0dajt7.jpg)

### 二叉查找树
定义：二叉查找树（BST）是一棵二叉树，其中每个结点都含有一个Comparable的键（以及相关联的值）且每个结点的键都大于其左子树中的任意结点的键而小于右子树的任意结点的键。

实现：

- 用内部私有类来表示二叉查找树上的一个结点；
- 每个节点都包含一个键、一个值、一条左链接、一条右链接和一个结点计数器；

代码：
![](https://ws4.sinaimg.cn/large/006tKfTcgy1fqv3pmkkl3j31dy18440x.jpg)

分析：
二叉查找树的运行时间取决于树的形状，而树的形状又取决于键被插入的先后顺序。最好的情况下，一棵含有 N 个节点的树是完全平衡的，每条空链接和根节点的距离都是 lgN。
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fqv3u104tij30ut0fa490.jpg)
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fqv3v1qm4tj30up05j0w8.jpg)

辅助函数：
每一个公有方法都对应着一个私有方法，接受一个额外的链接作为参数指向某一个结点。
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fqv51y5n2tj31ei11mgnk.jpg)
![](https://ws4.sinaimg.cn/large/006tKfTcgy1fqv52i6l9pj31ee0iagmm.jpg)

删除操作：

- 删除最小元素：不断深入根结点的左子树，直到遇到一个空链接，然后将指向该结点的链接指向其右子树。
- 删除最大元素：不断深入根节点的右子树，直到遇到一个空链接，然后将指向该结点的链接指向其左子树。
- 删除任意元素：删除拥有两个子结点的结点，考虑如何处理两棵子树？
	- 删除父结点后，用其后继结点代替，所谓后继结点就是右子树中最小的结点；
		- 将指向即将被删除的结点的链接保存为 t；
		- 将 x 指向它的后继结点 min(t.right)；
		- 将 x 的右链接指向 deleteMin(t.right)；
		- 将 x 的左链接指向 t.left；

代码：
![](https://ws2.sinaimg.cn/large/006tKfTcgy1fqv5y8nu3wj31eg0zctah.jpg)

范围查找：
中序遍历：遍历的顺序为 左子树 -> 根节点 -> 右子树；
思想：利用中序遍历，将所有落在指定范围内的键加入到一个队列中，并且跳过不可能满足条件的子树；使用 Iterable是为了使得队列支持foreach遍历操作；

实现：
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fqvkfbzz3qj31ec0h2jsj.jpg)

分析：
![](https://ws2.sinaimg.cn/large/006tKfTcgy1fqvkv680cmj30ur04mtbl.jpg)

答疑：

- 二叉树递归与非递归的优缺点？递归实现易于理解和验证正确性，非递归的实现效率更高。

### 平衡查找树
思想：使得含有 N 个结点的树，树高为 lgN，保证所有的查找都能在 lgN 次内结束；

#### 2-3查找树
2-结点：含有一个键和两条链接；
3-结点：含有两个键和三条链接；
![](https://ws2.sinaimg.cn/large/006tKfTcgy1fqvmks55cvj30ri07m78t.jpg)

图例：
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fqvmm51lj6j3098067jry.jpg)

完美平衡2-3查找树：树种所有空链接到根结点的距离都是相同的；

查找：
要判断一个键是否在树中，首先要和根结点的键比较，如果和其中任意一个相等，查找命中；否则根据比较结果找到指向相应去见的链接，并在其指向的子树中递归地继续查找，如果最后遇到了空链接，则说明查找未命中。

2-结点插入新键：
先进行一次未命中的查找，然后把新节点挂在树的底部，然后把2-结点替换成一个3-结点，将要插入的键保存在其中。

只含3-结点的树插入新键：
临时将新键存入3-结点，成为一个4-结点，然后转换为由3个2-结点组成的2-3树，其中一个结点（根节点）中键，一个结点包含最小键（和根结点的左链接相连），一个结点包含最大键（和根结点的右链接相连）。

向一个父节点为2-结点的3-结点插入新键：
临时将新键存入3-结点，成为一个4-结点，然后将中键调入父结点形成一个3-结点。
![](https://ws4.sinaimg.cn/large/006tKfTcgy1fqvv86350lj31hy0o249y.jpg)

向一个父结点为3-结点的3-结点插入新键：
临时将新键存入3-结点，成为一个4-结点，然后将中键放入父结点，父结点变为4-结点，然后继续往上直到寻找到一个2-结点。

分解根节点：
如果从插入节点到根节点的路径上全都是3-结点，那么根节点会变成一个4-结点，最后将根节点分解为3个2-结点，使得树高加1。

分析：
![](https://ws2.sinaimg.cn/large/006tKfTcgy1fqvvzqsoqij30uo04qmzp.jpg)

#### 红黑二叉查找树（参考算法导论，区别于书中内容）
思想：用标准的查找二叉树（完全由2-结点构成）和一些额外信息（替换3-结点）来表示2-3树，树种的链接分为两种类型：

- 红链接将两个2-结点连接起来构成一个3-结点；
- 黑链接是2-3树中的普通链接

定义：含有红黑链接并满足下列条件的二叉查找树：

1. 结点或者是黑色的，或者是红色；
2. 根结点是黑色的；
3. 每个叶子结点（此处指的是空节点）是黑色的；
4. 如果一个结点是红色的，则它的子结点必须是黑色的；
5. 从一个结点到该结点的子孙结点的所有路径上包含相同数目的黑结点；


应用：主要用来存储有序数据，时间复杂度为 O(lgn)。例如：Java集合中的`TreeSet`和`TreeMap`，C++ STL中的`set`、`map`，以及Linux虚拟内存的管理；

[基本操作](http://www.cnblogs.com/skywang12345/p/3245399.html)：

1. 左旋<br>
![](https://ws4.sinaimg.cn/large/006tKfTcgy1fqw2q56q2dj30b7065t8p.jpg)

2. 右旋<br>
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fqw2rta978j30b6067t8p.jpg)

3. 添加 put
	- 第一步：将红黑树当做一棵二叉查找树，将结点插入；
	- 第二步：将插入结点的颜色设为“RED”，不违背特性五；
	- 第三步：通过一系列旋转和着色操作，重新转换为红黑树；根据插入结点父结点的情况，分为三类：
		- 第一类：被插入的结点是根结点，直接将此结点涂成黑色；
		- 第二类：被插入的结点的父结点是黑色，什么也不需要做，插入后仍然是红黑树；
		- 第三类：被插入的结点的父结点是红色，与特性四相冲突，核心思想是：将红色结点移到根节点，然后根结点设为黑色，分为三种情况：
			- Case 1：当前结点的父结点是红色，且当前结点的祖父结点的另一个子结点（叔叔）也是红色的；处理：![](https://ws2.sinaimg.cn/large/006tKfTcgy1fqw3tlh4axj30mc07d0t2.jpg)
				- 将“父结点”设为黑色；
				- 将“叔结点”设为黑色；
				- 将“祖父结点”设为红色；
				- 将“祖父结点”设置为当前结点，继续操作；
			- Case 2：当前结点的父结点是红色的，叔叔结点是黑色的，且当前结点是其父结点的右子结点；处理：![](https://ws4.sinaimg.cn/large/006tKfTcgy1fqw3ul0vyoj30md06xjrr.jpg)
				- 将父结点设置为当前节点；
				- 以当前节点为支点左旋；
			- Case 3：当前结点的父结点是红色的，叔叔结点是黑色的，且当前结点是其父结点的左子结点；处理：![](https://ws1.sinaimg.cn/large/006tKfTcgy1fqw3v85e7vj30md06w74m.jp)
				- 将父结点设为黑色；
				- 将祖父结点设为红色；
				- 以祖父结点为支点右旋；

图例：将红黑树的红链接画平，所有的空链接到根结点的距离都是相同的。
![](https://ws3.sinaimg.cn/large/006tKfTcgy1fqvxkyefjnj30sh06kgog.jpg)

代码：
![](https://ws2.sinaimg.cn/large/006tNc79gy1fqy49zg2lsj30oz0qedgj.jpg)
![](https://ws3.sinaimg.cn/large/006tNc79gy1fqy4b48ibqj30p00b6aa9.jpg)

4. [删除 delete](https://blog.csdn.net/shltsh/article/details/46835757)
	- 第一步：将红黑树当作一棵二叉查找树，将结点删除，分三种情况：
		- 删除结点没有子结点，直接将该结点删除；
		![](https://ws2.sinaimg.cn/large/006tNc79gy1fqy5gwow01j30nw0bct8o.jpg)
		- 被删除结点只有一个子结点，删除后，用唯一子结点顶替其位置；
		![](https://ws4.sinaimg.cn/large/006tNc79gy1fqy5hn8pg3j30nc0baweg.jpg)
		- 被删除结点有两个子结点，先找出其后继结点，拷贝后继结点内容，删除后继结点；
		![](https://ws3.sinaimg.cn/large/006tNc79gy1fqy5k6b5kbj312o0bcaak.jpg)
	- 第二步：通过旋转和重新着色操作修正树，使之重新成为一棵红黑树：
		- Case 1：x是黑加黑结点，x的兄弟结点是红色；
			- 将x的兄弟结点设为黑色；
			- 将x的父结点设为红色；
			- 对x的父结点进行左旋；
			- 左旋后，重新设置x的兄弟结点；
		![](https://ws3.sinaimg.cn/large/006tNc79gy1fqy607eh86j318w0cajs9.jpg)
		- Case 2：x是黑加黑结点，x的兄弟结点是黑色，兄弟结点的子结点都是黑色；
			- 将x的兄弟结点设为红色；
			- 设置x的父节点为新的x结点；
		![](https://ws2.sinaimg.cn/large/006tNc79gy1fqy614kpb3j318w0cwwfe.jpg)
		- Case 3：x是黑加黑结点，x的兄弟结点的左孩子结点是红色，右孩子结点是黑色的；
			- 将x兄弟结点的左孩子设为黑色；
			- 将x兄弟结点设为红色；
			- 对x的兄弟结点进行右旋；
			- 右旋后，重新设置x的兄弟结点；
		![](https://ws1.sinaimg.cn/large/006tNc79gy1fqy61qpwjrj318s0eyt9l.jpg)
		- Case 4：x是黑加黑结点，x的兄弟节点的右孩子结点是红色，左孩子结点是任意颜色；
			- 将x父结点颜色赋值给x的兄弟结点；
			- 将x父结点设置为黑色；
			- 将x兄弟结点的右子结点设为黑色；
			- 对x的父结点进行左旋；
			- 设置x为根结点；
		![](https://ws4.sinaimg.cn/large/006tNc79gy1fqy62ivie1j31900dcjsf.jpg)
		
#### 散列表
用数组来实现无需的符号表，将键作为数组的索引而数组中键i处存储的就是它对应的值；

散列查找算法的步骤：

- 用散列函数将被查找的键转化为数组的一个索引；
- 处理碰撞冲突，典型方法有拉链法，线性探测法；

散列函数：对于正整数，浮点数和字符串可以使用除留余数法；

JAVA 的约定：每种数据类型都需要相应的散列函数，所有数据类型都继承了一个能够返回一个32比特整数的 hashCode() 方法，hashCode 方法和 equals 方法保持一致；

软缓存：如果散列值得计算很耗时，可以将每个键的散列值缓存起来，即在每个键中使用一个hash变量来保存它的hashCode返回值；

优秀散列方法需要满足的三个条件：

- 一致性：等价的键必然产生相等的散列值；
- 高效性：计算简便；
- 均匀性：均匀地散列所有的键；

![](https://ws3.sinaimg.cn/large/006tNc79gy1fqyb05wyo9j311b09hahq.jpg)

拉链法：将大小为M的数组中的每个元素指向一条链表，链表中每个元素指向一条链表，链表中的每个结点都存储了散列值为该元素的索引的键值对。

代码：
![](https://ws2.sinaimg.cn/large/006tNc79gy1fqyc57oitgj31e00oggmv.jpg)

分析：
![](https://ws1.sinaimg.cn/large/006tNc79gy1fqyen78hg6j31180dy14a.jpg)

线性探测法：用大小为M的数组保存N个键值，其中 M>N，依靠数组中的空位解决碰撞冲突，当碰撞发生时，直接检测散列表的下一个位置。

代码：
![](https://ws2.sinaimg.cn/large/006tNc79gy1fqyfrqa666j31e011g0uj.jpg)

删除操作：线性探测法不能直接删除元素，不然会导致在此元素之后的元素无法被探测到，所以在此元素之后的元素需要被删除然后重新插入；

箭簇：线性探测的平均成本取决于元素在插入数组后聚集成的一组连续的条目，称为箭簇，短小的箭簇可以保证较高的插入效率。

分析：
![](https://ws2.sinaimg.cn/large/006tNc79gy1fqyfzkrt62j30tj0o8h1a.jpg)

内存分析：
![](https://ws4.sinaimg.cn/large/006tNc79gy1fqyg2k1gufj30u205m76b.jpg)

答疑：

- 问：JAVA中Integer，Double，Long类型的hashCode()方法是如何实现的？
- 答：Integer会返回该整数的32位值，对于Double和Long会返回机器表示前32位和后32位抑或的结果。
- 问：动态调整数组大小时大小总是乘以2，会不会只使用了hashCode的低位值？
- 答：是的，解决的一个方法是用一个大于M的素数来散列键值对。
- 问：为什么不将hash(x)实现为x.hashCode%M, Math.abs(x.hashCode())%M?
- 答：散列值必须在0到M-1之前，在Java中取余的结果可能会是复数；而Math.abs()在对最大整数时会返回一个负数。
- 问：散列表的查找会比红黑树快么？
- 答：取决于键的类型，决定了hashCode()的计算成本是否大于compareTo()的比较成本。对于常见类型，两者计算成本类似，因此散列表比红黑树快得多。


## 图
