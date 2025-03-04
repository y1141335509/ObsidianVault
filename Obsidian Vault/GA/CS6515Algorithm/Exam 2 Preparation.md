## 考试内容
1. 2 个 Graph Algorithm Design Problems （每个20分）这些问题还是类似于Homework的解答方式
2. 一组选择题/填空题 （总分20）。这些题涵盖所有方面

## 考试须知
1. 对于任何black-box 算法的内部改动都会导致无法获得full credit
2. 对于算法需要用文字进行解释，而不是伪代码。
3. 需要提供正确的算法设计，算法陈述 和 算法时间复杂度描述
4. 考试将于周四上午十点（东部时间）开放，周一早上八点（东部时间）关闭

## 考试范围
1. Week 6 - Graph traversal and connectivity - Videos [GR1](https://edstem.org/us/courses/70186/lessons/126465) + [GR2](https://edstem.org/us/courses/70186/lessons/126466). Format Quiz on Graph Algorithms
2. Week 7 - Greedy algorithms and structures - Shortest path and MST. Videos [DP3](https://edstem.org/us/courses/70186/lessons/126459) + [GR3](https://edstem.org/us/courses/70186/lessons/126467)
3. Week 8 - Flows on networks - Videos [MF1](https://edstem.org/us/courses/70186/lessons/126469) + [MF2](https://edstem.org/us/courses/70186/lessons/126470) + [MF4](https://edstem.org/us/courses/70186/lessons/126472). Content Quiz on Graph Theory


## GR1 - SCC
SCC 定义：在有向图中，如果一个图的每个顶点都可以从其他顶点到达，那么这个图就是强连通的。例如下面的节点3 可以经过节点0 到达节点1。所以0，1，2，3组成一个强连通。
![[Pasted image 20250304095300.png]]
找到一个Graph中的SCCs只需要run两次DFS就可以了
要找到DAG

![[Screenshot 2025-03-04 at 10.07.10.png]]

**当且仅当DFS tree上存在至少一个 back edge的时候，图G（有向图）才存在cycle**

**DAG = Directed Acyclic Graph 有向无环图。无环也就意味着没有 back edge** - 

Topological Sort一个DAG - 意味着将所有节点按照他们的post顺序 由高到低 进行排序。具体来说，我们run一个DFS on DAG G，对于所有的edge z -> w，我们知道 post(z) > post(w)，所以我们按照 z, w, ... 的顺序进行排序，也就是拓扑序。runtime = O(n+m)。此外要注意，Topological Sort是无法在 非DAG图上运行的

Source Vertex = no incoming edges
Sink Vertex = no outgoing edges
在下面的图中，Z W都可以是Sink Vertex。
![[Screenshot 2025-03-04 at 10.34.59.png]]
![[Screenshot 2025-03-04 at 10.35.33.png]]
除了使用Topological Sort之外，另一种确定Topological order的方法是：
1. 先找到一个Sink，然后删掉它
2. 找到下一个Sink，删掉它
3. 。。。
4. 直到找到那一个Source为止


**Undirected Graph中的Strongly Connected Component （SCC）类比于 Directed Graph中的Connected Component**
SCC定义：
SCC 定义：在有向图中，如果一个图的每个顶点都可以从其他顶点到达，那么这个图就是强连通的。例如下面的节点3 可以经过节点0 到达节点1。所以0，1，2，3组成一个强连通。
![[Pasted image 20250304095300.png]]
**MetaGraph** - 如下图，我们先将一个有向图的SCC都用红圈 标记出来，然后将每个SCC中的节点写出来放在下面绿色椭圆中。然后将连接每个SCC的edge用最下面的黑色箭头标记。这样就形成了一个Metagraph。当然，某两个SCC之间可能会有两个不同的edge将它们连接。此时我们可以用两个黑色箭头标记。但通常为了简化，我们可以省略重复的黑色箭头。
![[Screenshot 2025-03-04 at 11.09.56.png]]
**Metagraph中你会发现这里面没有 Cycle。所以所有的Metagraph都是一个DAG**

* **在DAG中 拥有lowest post-order的节点 一定是一个 sink节点**；**在DAG中 拥有highest post-order的节点 一定是一个 source节点**
* 在一个普通的Directed Graph中，**不能保证** 拥有lowest post-order的节点一定是一个sink节点；在一个普通的Directed Graph中，**可以保证** 拥有highest post-order的节点一定是一个source节点
* 

**如何找到一个Directed Graph中的SCC？**
首先引入一个概念就是“反图” （Reverse Graph），它表示将$G(V, \vec{E})$, where $\vec{E}=\{v, w\}$的所有边的方向 倒过来，也就是$G(V, \vec{E})$ , where $\vec{E}=\{w, v\}$。
上面的图进行反图，就得到了下面的图。然后随便选一个节点（例如节点C）进行DFS遍历，记录下pre 和post order如下图。然后在剩下未遍历的节点中随便选一个（节点D）记录下pre 和post order。以此类推，直到所有节点都被遍历完为止。就得到所有节点的pre 和post order。
![[Screenshot 2025-03-04 at 12.01.33.png]]
有了所有节点的pre和post order之后，我们按照post order降序排列：L -> K -> J -> I -> H -> D -> C -> B -> E -> A -> G -> F。
然后回到原本的Graph G上，按照该顺序run DFS。首先从L开始，会遍历完L, K, J, I, H。这五个节点就是第一个SCC。然后遍历D，它自己是第二个SCC。然后是C, G, F，它们3个是第三个SCC。然后是B, E，它们是第四个SCC。最后是A，它自己是第五个SCC。
![[Screenshot 2025-03-04 at 13.34.35.png]]
































