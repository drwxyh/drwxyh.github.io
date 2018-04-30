---
layout:     post                    # 使用的布局（不需要改）
title:      笔记：算法复习 			   # 标题 
subtitle:   重新学习算法的一些笔记	   # 副标题
date:       2018-04-27              # 时间
author:     Charles Xu              # 作者
header-img: img/post-bg-space-0.jpg    #这篇文章标题背景图片
catalog: false                       # 是否归档
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