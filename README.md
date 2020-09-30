【八数码问题】
=====
                 
在3×3的棋盘上，摆有八个棋子，每个棋子上标有1至8的某一数字。棋盘中留有一个空格，空格用0来表示。空格周围的棋子可以移到空格中。要求解的问题是：给出一种初始布局（初始状态）和目标布局（为了使题目简单,设目标状态为123804765），找到一种最少步骤的移动方法，实现从初始布局到目标布局的转变。

实验过程
--------
在解决八数码问题时使用C++编程，主要用以下四种方法进行探究解决：
* 广度优先搜索
* 深度优先搜索
* 深度迭代搜索
* 启发式搜索

以上四种方法的代码有参考一些资料和CSDN上一些大神的代码，研究后结合代码编写提交
下面是实验过程中我对这四种方法的理解

广度优先搜索
----------
时间复杂度1+n+n^2+… +n^q=O(n^q+1)

空间复杂度O(n^q+1)

使用两种方法编写了BFS代码，主要是数组及队列

广度优先搜索可以得到最短路径的解，但是当目标节点距离初始节点较远时会产生许多无用的节点，搜索效率低。

深度优先搜索
-------------
时间复杂度O(bm): 如果 m比d大很多则比较严重

空间复杂度O(bm), 线性空间

在编写深度优先搜索方法时对于算法的构建一直有些问题，对于深度优先算法，如果目标节点不在搜索所进入的分支上，而该分支又是一个无穷分支，则就得不到解.因此该算法是不完备的，在代码的构建上参考了一些大神代码，理解编程。

迭代深度搜索（IDS）
-------------
迭代深度索搜是深度优先搜索的一个优化版，普通深度优先搜索是一直往下扩展，这样可能会陷入无限循环，IDS是有界的深度搜索，当搜索到一定深度的时候若还没找到解就不再往纵向搜索了，转而向横向搜索。这样，可以先设深度为1，然后迭代的递增深度，若在当前深度搜不到解就继续迭代下一个深度，这样如果问题是有解的，那么总能找到解，且是最优解。因为这种思想类似于广搜，一层一层的迭代，最优解总在较浅层找到。

【代码方面的思考】

首先将初始节点和节点深度（初始节点深度为0）配成对入队（栈）。然后遍历这个栈，将栈顶出栈，先判断一下是否达目标状态，没达到的话若深度还没达到最大深度，则扩展，将可扩展的节点入栈，这样下次出栈的时候操作的就是这次最后入栈的节点。若全部搜索完都找不到解则返回-1，此时再迭代下一个深度，重复前面的过程。若状态有解，则IDS一定能找到解，因为深度是可迭代加深的，且是最优解，因为在浅层先找到的解代价最小。

A*算法
-------------
A*搜索：总代价 f(n) = g(n) + h(n)，其中g(n)为从初始状态到达该状态的代价，h(n)为从当前状态到目标状态的预估代价。每次寻找总代价f(n)最小的点进行扩展，直到找到终点。

【代码方面的思考】

首先生成随机排列的整数，然后检查生成的随机序列是否有解，若有解，进行A*算法，

在运用A*算法时关键在于运用启发式函数，因为要进行评估，这里的启发式函数构建思路如下：

* 计算有多少个不在正确位置上的数码的个数来作为当前节点和目标节点的相似性度量
* 为每个错误放置的数码，计算其到正确位置的城市距离，由这些距离之和作为相似性度量，也就是估算代价h。
