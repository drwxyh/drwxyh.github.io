---
layout:     post                    # 使用的布局（不需要改）
title:      思考：如何写论文？    # 标题 
subtitle:   来自知乎回答的整理			   #副标题
date:       2018-04-24              # 时间
author:     Charles Xu              # 作者
header-img: img/post-bg-paper-1.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - 论文
    - 写作技巧
---
#  How to Write Technical Articles 
论文可以被分为两类：一类是原创研究论文，另一类是调查论文。

####  研究型论文
- 好论文的定义：
	- 需要对*论文解决的问题、提出的解决方案、得到的结果*有个清晰的陈述；
	- 需要描述 *问题已有的研究 和 本文的创新点*；
- 论文的目的：描述创新技术的结果，主要有四种类型
	- 新的算法
	- 系统结构，如：硬件设计，软件系统，协议等
	- 绩效评估，通过分析，模拟或者测量得到
	- 新的理论，由一组定理组成
- 论文的关注点：
	- 提供充分的细节来描述结果，以验证其有效性
	- 确定论文的新颖点，涉及到什么新知识，什么方面做得不够好
	- 确定论文结果的重要性，提出了什么样的改进措施，有什么效果

####  论文的传统框架
建议先写 approach and result section，然后写 problem section，然后 conclusions, 然后 introduction。后写 introduction 是因为它需要包括总结中的内容。最后写 abstract，给全文一个简明扼要的总结。

- *Abstract*：一般来说 *100~150* 字，不要太多。
- *Introduction*：介绍问题、列出解决方案。问题的陈述需要包括叙述研究问题的重要性。 
- *Outline The Rest of the Paper*： 注意句式的多样性，比如：
	- The remainder of this paper is organized as follows.
	- In Section 2, we introduce……
	- Section 3 discusses……
	- Section 4 describes……
	- Finally, we describe future work in Section 5.
- *Body of Paper*：至少要有一个实例场景(最好两个)，有说明性的图表，清晰的问题模型陈述，侧重于“新”功能，对于算法或者结构的通用评估，例如证明你的算法是'O(logN)'。
	- problem model
	- approach or architecture : 基于问题模型提出的系统结构应当比你实现的系统更加通用，通常需要包括至少一张图；
	- results : 实现：需要包括实际的实现细节，但并非从头到尾铺陈描述，简要提及：实现语言，平台，位置，依赖包和资源使用情况；评估：在实际使用中如何运行？提供真实或者模拟的性能指标和终端用户研究。
- *Related Work*：对于会议文章来说，尽量引用会议主席团及其他成员的成果，对于期刊或者杂志来说，尽量引用最近 2~3 年的相关内容。
- *Summary and Future Work*：重复主要的研究结果
- *Acknowledgements*：研究经费支持者
- *Bibliography*
- *Appendix*: 
	- 详细的协议描述
	- 详细的证明
	- 其他低层次但是重要的细节

####  标题-Title
- 避免使用缩写，但是可以包括常见的易于理解的缩写；
- 避免常见短语，比如：“novel”、“performance evaluation” 或者 “architecture”，因为这些单词太常见了，一搜索就是几千条结果，没人会拿这种词作为搜索关键词；用一些能体现你工作特征的形容词，比如：reliable，scalable，high-performance，robust，low-complexity，low-cost。
- 如果需要论文标题的灵感，可以查询 [Academic Essay Title Generator](https://www.besttitlegenerator.com/index.php) 或者 [Research title generator!](https://cjquines.com/titlegen#) 之类的网站。

####  作者-Authors
基于IEEE标准，被列入作者的个人需要满足以下条件：
- 为理论发展、系统设计、实验设计、原型开发、分析和解释论文中数据做出重要智力贡献；
- 为文章起草，审查文章、修改内容做出贡献；
- 参与文章最终版本（包括参考文献）的确定工作；

####  摘要-Abstract
- 摘要不需要包含参考文献，摘要常常会独立于论文主体使用；
- 避免使用 *In this paper* 短语，因为在这一块你不会谈其他文章；
- 不需要证明一些常识，比如：因特网的重要性，Qos是什么东东；
- 不仅要突出问题，也要突出结果，因为很多人通过阅读摘要决定读不读全文；
- 因为摘要会被搜索引擎检索，所以摘要中需要包含能标志你工作的术语，特别是开发的协议和系统名称之类；
- 不要包含数学公式；

####  简介-Introduction
- 避免陈词滥调，例如：“recent advance of X” 或者任何暗示互联网发展的话；
- 要让读者了解你的论文是关于什么的，不要光说你的研究领域如何如何重要，读者不会坚持三页来搞清楚你要干什么；
- 简介中必须通过指出你正在解决的问题来彰显你工作的必要性，概括你的方法或者贡献，给出论文结果的一般性描述；
- 不需要重复摘要中的内容；
- 反面示例: <br><br>
Here at the institute for computer research, me and my colleagues have created the SUPERGP system and have applied it to several toy problems. We had previously fumbled with earlier versions of SUPERGPSYSTEM for a while. This system allows the programmer to easily try lots of parameters, and problems, but incorporates a special constraint system for parameter settings and LISP S-expression parenthesis counting. The search space of GP is large and many things we are thinking about putting into the supergpsystem will make this space much more colorful.<br><br>

- 正面例子：<br><br>
Many new domains for genetic programming require evolved programs to be executed for longer amounts of time. For example, it is beneficial to give evolved programs direct access to low-level data arrays, as in some approaches to signal processing \cite{teller5}, and protein segment classification \cite{handley,koza6}. This type of system automatically performs more problem-specific engineering than a system that accesses highly preprocessed data. However, evolved programs may require more time to execute, since they are solving a harder task.<br><br>
<b>Previous or obvious approach:</b><br/>
(Note that you can also have a related work section that gives more details about previous work.) One way to control the execution time of evolved programs is to impose an absolute time limit. However, this is too constraining if some test cases require more processing time than others. To use computation time efficiently, evolved programs must take extra time when it is necessary to perform well, but also spend less time whenever possible.<br><br>
<b>Approach/solution/contribution:</b><br>
The first sentence of a paragraph like this should say what the contribution is. Also gloss the results.
In this chapter, we introduce a method that gives evolved programs the incentive to strategically allocate computation time among fitness cases. Specifically, with an aggregate computation time ceiling imposed over a series of fitness cases, evolved programs dynamically choose when to stop processing each fitness case. We present experiments that show that programs evolved using this form of fitness take less time per test case on average, with minimal damage to domain performance. We also discuss the implications of such a time constraint, as well as its differences from other approaches to {\it multiobjective problems}. The dynamic use of resources other than computation time, e.g., memory or fuel, may also result from placing an aggregate limit over a series of fitness cases.<br><br>
<b>Overview:</b><br>
The following section surveys related work in both optimizing the execution time of evolved programs and evolution over Turing-complete representations. Next we introduce the game Tetris as a test problem. This is followed by a description of the aggregate computation time ceiling, and its application to Tetris in particular. We then present experimental results, discuss other current efforts with Tetris, and end with conclusions and future work.

- 童话写作法：
	1. 有一条巨龙抓走了公主（这个问题为什么值得研究？）
	2. 巨龙多么多么的难打（强调你研究的重要性）
	3. 王子提着一把金光闪闪的剑而不是破斧子烂长矛登场（你的方法好在哪里，别人的差在哪里）
	4. 王子是如何打败巨龙的（你的方法的简介）
	5. 从此王子和公主幸福的生活在一起（解决了问题）

####  主体-Body of Paper
在动笔写之前，一定要搞清楚自己想写的是什么。需要重视的不是所用的字词，如果你在为了遣词造句抓耳挠腮的话，准备废弃你写的东西吧，即使它是一整篇文章。下面是一些需要注意的原则：

- 避免使用被动语态，比如：“In each reservation request message, a refresh interval used by the sender is included.” 最好改成 “Each ... message includes ...”
- 使用强大的动词替换大量的名词，多使用简单术语，少用太花哨的，比如：<br>

	Advocate      | Avoid
	------------- | -------------
	assume        | make assumption
	depends on    | is a function of
	illustrates,shows|is an illustration
	requires, need to|is a requirement
	uses|utilizes
	differed|had difference
- 不用使用 “In other words”，用到了就说明你前面说的不够清楚，回过头去重写；
- 不要误用名词，尤其是母语中没有的词，不要在固定术语和名字前面添加名词。
- 句子之间应该存在逻辑关系，注意使用逻辑连词：
	- 转折 describe an exception : but, however
	- 因果 describe a causality : thus, therefore, because of this
	- 正反 indicate two facets of an argument : on the one hand, on the other hand
	- 罗列 enumerate sub-cases : firstly, secondly
	- 时序 indicate a temporal relationship : then, afterwords
- 协议的名称缩写通常不被视作名词，但是组织名称缩写被视作确定的名词；
- 除非是在早期论文中获得的结果，否则通常使用一致的现在时态；
- 连字符的使用：
	- 加前缀，如：pre-,co-,de-,pro-
	- all-around
	- self-conscious
- 不要过度使用破折号分割；
- 不要乱使用引号，因为会导致读者认为单词具有讽刺意味；
- 十以下的数字尽量拼写，不要使用阿拉伯数字；
- 使用 $$Eq. 7$$，不用 $$Eq (7)$$;
- 避免在行中列举，例如：“Packets can be (a) lost, (b) stolen, (c) get wet.” 除非你之后要引用；
- 大多数情况下，避免使用分项（子弹头），因为会占据额外空间，而且看上去更像PPT；
- 使用 “Smith [1] showed” 或者 “Smith and Jones [1] showed”，而不是“Reference [1] shows” 或者 “[1] shows”，超过两位作者时，使用 “Smith et al. [1] showe”；正确引用：“Smith and Wesson claim that ... [42]” 或者 “In [42], Smith and Wesson claim that..”；
- 在说明中使用正常大小写，如："This is a caption," 而不是 "This is a Caption"；
- 所有级别标题统一大写，风格保持一致；
- 圆括号或者括号都要有一个空格，如："The experiment (Fig. 7) shows" ；
- 避免过分使用括号，会影响阅读，能使用脚注尽量使用脚注，但是脚注一般一页也不要超过两条；
- 缩写要先声明，再使用；
- 句子不要以 “and” 或者 “That's because……” 开头；
- 不要在句中使用冒号，冒号之前应当是一个完整的句子，反例：“This is possible because: somebody said so”；
- 在正式写作中，通常应当避免“don't, doesn't, won't”；
- 比较的表达方式，"Flying is faster than driving" 的表达比 “Flying has the advantage of being faster” 好；
- 不要使用“/”，应当使用 or 或 and 替代，“+”，“%”，“->”应当用相应单词表示；
- 技术术语应当小写，除非在解释缩写时可以大写首字母；
- 每一段都应当有一个统领句概括整段，除非段落太短。应当能保证只看段落首句的情况下，论文仍能表达完整意思。例如：<br><br>
There are two service models, integrated and differentiated service. Integrated service follows the German approach that anything that isn't explicitly allowed is verboten. It strictly regulates traffic, but also makes the trains run on time. Differentiated service follows the Animal Farm appraoch, where some traffic is more equal than others. It seems simpler, until one has to worry about proletariat traffic dressing up as the aristocracy.

- 使用 $$i$$th，而不是 $$i-th$$；
- 为了提高可读性，1,000 应当用“,”分开；
- 单位使用罗马字体，不使用斜体或者数学模式；
- 使用 "kb/s“ 或者 ”Mb/s“，而不是 "kbps"，"Mbps"，因为它们不是科学单位，注意 ”kb“(1000 bits) 和 ”KB“(1024 bytes)的区别，注意 ”kHz“，”Wi-Fi“；
- 操作系统，如：”Android, OS X, Linux“，应当大写；
- 小数点前的 ”0“ 不能省略；
- 使用”for example, such as, among others“ 比 ”etc.“ 好，除了引用，尽量全部列全。使用 ”for example“ 和 ”like“ 不要跟着 ”etc.“；
- ”i.e.“ 和 ”e.g.“ 后面总是跟着逗号；
- 使用”In Figure 1“ 而不是 ”the following Figure“，因为排版图的位置会改变；
- 表格中的文本左对齐，数字按小数点对齐或者右对齐；
- Section, Figure, Table大写；
- 表示变量之间关系是使用线图，线不能依靠颜色区分，通常论文黑白打印。显示不同实验时使用柱状图或者散点图；
- 引用的格式需要保持一致，会议论文引用要加上会议地址，时间，会议全称(不是缩写)；期刊需要加上卷号、期号和页码；年份在某一部分出现一次即可，不要重复出现；

####  参考书目-Bibliography
- 作者避免使用……等，除非作者多于五位；
- 互联网草案必须标注为”work in progress“；
- 书籍引用需要包括初版年份，但是不需要 ISBN(国际标准书号)；
- 引用中不应当出现URL地址；
- 外国人姓名之间应当空格，如："J. P. Doe"；

####  致谢-Acknowledgements
- 感谢资金支持者，有的要特殊格式，例如："This work was supported by the National Science Foundation under grant EIA NN-NNNNN."
- 通常匿名审稿人不被感谢，除非真的提供了特别大的帮助，使用”X helps with Y.“句型陈述；

####  实验结果-Reporting Numerical Results and Simulations
- 需要披露足够多的实验细节，保障阅读者可以重复试验，需要提供所有使用的参数，分析使用的样本数量；
- 模拟结果应当有置信度，如果可能，提供置信区间，存在异常数据时，应当解释异常数据出现的原因；‘
- 绘图应当有选择，选择哪个参数在什么区间绘图，出现大量直线是建议的；
- 对于图的描述不应当是表面的，应当具体描述变量之间的关系，是线性的，还是其他关系？

####  LaTeX的使用
- 没有必要把数字包装在 ”$$“ 中；
- 使用”\cite{1,2,3}“ 而不是 ”\cite{1} \cite{2} \cite{3}“；

####  会议论文审查过程
1. 会议论文以 pdf 格式递交给委员会；
2. 技术委员会主席将论文发给一至多位技术委员会成员审查，技术委员会成员身份保密；
3. 审稿人反馈一份审查报告，通常是3份；
4. 委员会整理审查报告，将论文按得分排序，开会审议，通常前三分之一直接接受，后三分之一直接拒绝，中间的需要进一步讨论；

####  论文的撰写流程
1. 搜集资料，查找相关论文；
	- 文献查阅报告，阅读主要文献在40篇以上；
	- 选题报告书
		- 为什么研究这个问题（背景、目的、意义）
		- 国内外研究现状
		- 研究内容、解决的关键问题
		- 怎样研究和解决这些问题
	- 预期的研究成果和创新点，研究进度安排
2. 学习相关理论，以及分析计算软件；
3. 初步确定论文的总体框架；
4. 完成论文课题的主体部分；
5. 分块撰写论文；
6. 注意图、表、公式、参考文献格式；

####  参考文献
- [硕士论文你有哪些经验与收获？](https://www.zhihu.com/question/20141321/answer/14112630)
- [Writing Technical Articles](http://www.cs.columbia.edu/~hgs/etc/writing-style.html)