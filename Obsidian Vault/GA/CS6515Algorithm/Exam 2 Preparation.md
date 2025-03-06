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
![[Screenshot 2025-03-04 at 13.37.53.png]]
该算法不仅仅是能够找到所有的SCC而已，如果我们画出Metagraph的话会发现：1-5 SCC之间的edge都是从右向左的（如上右图）。也就是**反向的Topological Order**。

总结一下：
**要找到一个普通有向图中的SCC，我们需要run两次DFS。第一次DFS跑在reverse Graph上；第二次DFS跑在原Graph上。我们最终可以找到所有的SCC，且所有 SCC之间的边都是从右向左连接的（也就是反向的Topological Order）。**
![[Screenshot 2025-03-04 at 13.42.52.png]]

Dijkstra - $O((n+m) \log n)$ runtime, $n$ is the number of vertices; $m$ is the number of edges


## GR2 - SAT
给了一个formula，例如$f=(X_1\lor X_2)\land(\bar{X_1}\lor X_2)$ ，已知$f$为真，求每个$X$的真假。
对于该类型的问题有如下事实：
* 如果对于所有$i， X_i和\bar{X_i}$ 都在不同的SCC中，那么*S是一个Sink SCC* 和*Component $\bar{S}$是一个Source SCC* 这两个陈述之间是 充要条件。
 ![[Screenshot 2025-03-04 at 14.28.38.png]]
课件里的做法就是构建了一个有向图，然后找到图中的Source和Sink节点。将Sink节点$S$变成True，同时将Source节点$\bar{S}$变成False；然后去掉当前的Sink节点$S$和Source节点$\bar{S}$。重复，直到没有任何SCC为止


## DP3 - Shortest Paths

DPV:
6.17 (change making)
6.18
6.19
6.20 (optimal BST)
6.7 (Palindrome subsequence)


**Shortest Path Algorithms via DP** - 给定一个有向图$\vec{G}=(V,E)$，它的edge的权重为$w(e)$。为了方便，我们总是用$s$表示起点，$z$表示终点。$\text{dist}(z)$表示从$s$ 到$z$的最短路径。比较常用的**Dijkstra's Algorithm的局限在于：1）只能解决单源的最短路问题；2）要求所有edge的权重不能为负。**该算法的时间复杂度为 $O((n+m)\log n$

当图中存在cycle，且该cycle的所有edge 权重之和为负的时候，Dijkstra algorithm会无限次在该cycle中循环。

现在我们假设某个图，没有这样的negative cycle。这意味着从$s$ 到$z$的最短路径$p$会经过图中每个节点最多一次。进而意味着 $|p|<=n-1$ edges。
DP idea -> 我们想要用 $DP(i-1,z)$来表示$DP(i,z)$。其中$i, z$分别表示第$i$个edge和第$z$个节点。
看下面的图，要想找到递推公式，我们可以想象从$s$ 到$z$，在到达$z$之前，我们假设要找到一个节点$y$ 满足从$s$ 到$y$也是最短路，且从$y$ 到$z$也是最短的。**这意味着$DP(i,z)=\min_{\vec{yz}\in E}\{DP(i-1,y)+w(y,z)\}$。此外还要注意要取该值与$DP(i-1,z)$之间的最小值**。
![[Screenshot 2025-03-05 at 10.31.00.png]]
**所以我们有如下完整的递推公式：**
$DP(i,z)=\min\{DP(i-1,z), \min_{\vec{yz}\in E}\{DP(i-1,y)+w(y,z)\}\}$
![[Screenshot 2025-03-05 at 10.37.45.png]]

### Bellman Ford Algorithm
![[Screenshot 2025-03-05 at 10.42.10.png]]
**$i$表示边的个数**
**通过上面的算法，如何找到Negative Weight Cycle？**
![[Screenshot 2025-03-05 at 10.52.39.png]]
首先由于从 s 到 a 权重为5，所以在(i=1, a)处填5；该行剩下列都是没有路径的，所以填∞。
接着从s 到 b 是 5 + 3，所以填8；该行剩下列都是没有路径的，所以填∞。
![[Screenshot 2025-03-05 at 10.54.43.png]]
再继续看，从b到c权重为-6，所以在(i=3, c)列 填 8+ (-6) = 2；此外 从 b 还可以到 d，所以在 (i=3, d)列填 8+4=12
![[Screenshot 2025-03-05 at 10.58.57.png]]
然后(i=4, e)要注意，是要从 c -> e，我们发现从 s 到 c的最短路径为2，所以在(i=4, e) 填 2 + 5 = 7。
然后(i=4, a)也要注意，我们可以从s -> a -> b -> c -> a，也就是在negative weight cycle处兜了一圈。所以(i=4, a)填 5+3+(-6)+2 = 4
剩下的格子(i=4, b), (i=4, c), (i=4, d)都不变，分别是8，2，12 （如下图）
![[Screenshot 2025-03-05 at 11.05.22.png]]
下面是完整的DP table
![[Screenshot 2025-03-05 at 11.16.50.png]]
**当图中存在negative weight cycle的时候，当经过它的时候，当前的 i 行 会与 i-1 行不同**。如果没有negative weight cycle的时候，我们的算法会在 i=5 行就结束。但有negative weight cycle的话，需要 i=6 行，且 i 行 会与 i-1 行的值不同。
**那如何确定到底是哪几个节点组成了该negative cycle呢？** - **Check if $DP(n,z)< DP(n-1,z)$ for some $z\in V$**。例如，(i=4, a) < (i=3, a)，所以a 是；(i=5, b) < (i=4, b)，所以b 是；(i=6, c) < (i=5, c)，所以c 是；

以上就是具体的计算案例。对于Bellman-Ford algorithm，它的时间复杂度是 $O(nm)$, $n, m$分别是节点个数和edge个数。该算法是单源最短路算法。如果要让计算所有节点对 之间的最短路，那就是要运行 n 次该算法，也就是 $O(n^2m)$的复杂度。之后我们将讨论Floyd-Warshall 算法，它的复杂度是$O(n^3)$。通常来说 $m$的数量级约等于$n^2$，所以 Floyd-Warshall算法的复杂度通常是 快于 $O(n^2m)$的。


### Floyd-Warshall Algorithm
**该算法的思想也是DP，但是一个3维DP。$DP(i,s,t)$。其中，$i,s,t$分别表示：$s,t$之间的中间节点；起点；终点。所以$DP(i,s,t)$表示从s 到 t 之间，可能会经过 ${1,2,..., i}$作为中间节点的最短路径$P$。**
Base Case如下图：
![[Screenshot 2025-03-05 at 11.41.10.png]]
递推公式：也就是当 第i 个节点$i\notin P$的时候，$DP(i,s,t)=DP(i-1,s,t)$
当第i 个节点$i\in P$的时候，如下图，我们的最短路径应该是从 s -> $\{1,2,...i-1\}$节点集合 -> 节点$i$ -> $\{1,2,...i-1\}$节点集合 -> 终点 t
![[Screenshot 2025-03-05 at 11.53.56.png]]
**所以$i\in P$的递推公式应该是： $DP(i,s,t)=DP(i-1, s,i)+DP(i-1,i,t)$** —— 也就是多了一段**从 s 到 i （但是可能经过了 {1,...i-1}节点集合）的路径** 加上 **从 i 到 w （但是可能经过了 {1,...i-1}节点集合）的路径）**
最终我们给出该算法的递推公式为： 
$DP(i,s,t)=\min\{DP(i-1,s,t),DP(i-1,s,i)+DP(i-1,i,t)\}$
![[Screenshot 2025-03-05 at 12.27.54.png]]
该算法也预设所有权重都是非负的。那如果有负权重该如何找到呢？——我们看下面的例子就能发现，如果我们计算$DP(n,a,a)=-1$。也就是说从一个节点到该节点本身，在经过了最多 n 个中间节点之后，这条最短路径的值 是负数。**所以我们可以检查 DP table的对角线元素，如果任何一个元素的 DP 值 为负，那就存在着 negative cycle**, i.e., Check if $DP(n, y, y) < 0$ for some $y \in V$.
![[Screenshot 2025-03-05 at 12.30.54.png]]

对比一下Bellman-Ford和Floyd-warshall这两种算法（我们还用上图为例）：
* Bellman-Ford - 假设我们选择了 d节点作为 起始节点 s 。**那么该算法其实并不能够找到negative cycle 或者例如 e 节点这样 unreachable 节点的最短路径的**。所以**该算法仅限于 从起始节点s 到 reachable 节点的单源最短路径查找**
* Floyd-Warshall- 并没有上面Bellman-Ford算法的限制。它能找到所有节点对（pair-wise）之间的最短路径



## GR3 - Minimum Spanning Tree (MST)
**Given** - undirected $G=(V,E)$ with weights $w(e)$ for $e\in E$.
**Goal** - find the minimal size, connected subgraph (i.e., spanning tree) of min weight

什么是Tree？Tree = Connected, Acyclic Graph （连通无环图）
Tree的基本性质是：
1. Tree on $n$ vertices has $n-1$ edges
2. In a tree, exactly 1 path between every pair of vertices
3. Any connected $G=(V,E)$ with $|E|=|V|-1$ is a tree


![[Screenshot 2025-03-05 at 20.56.47.png]]


图的切分性质（Cut Property）：
引理 - For undirected graph $G=(V,E)$, take $X\subset E$, where $X\subset T$ for a MST $T$.  然后取$S\subset V$ where no edge of $X$ is in the $\text{cut}(S,\bar{S})$. 检查所有$G$的Cut 的所有edges。让 $e^*$ 表示该$\text{cut}(S, \bar{S})$所有edges中权重最小的那个。那么$X \cup e^*\subset T'$ where $T'$ is a MST.
可能不太好理解，**我们看下图，假设我们将一个Graph 切分成了左右两个部分$S, \bar{S}$。 $e^*$ 表示该$\text{cut}(S, \bar{S})$所有edges中权重最小的那个，那么根据MST的定义不难发现$e^*$也应该被算作是最终 MST $T$ 的边 之一。如此一来就有了递推公式**
![[Screenshot 2025-03-05 at 21.13.46.png]]



## MF1 - Ford-Fulkerson Algorithm
假设有一个Flow Network，它是一个directed graph $G=(V, E)$, source节点为$s$, sink节点为$t$， $s, t \in V$。此外 对于每一个边$e\in E$，且每个edge都有一个capacity $c_e >0$。如下图：
![[Screenshot 2025-03-05 at 21.32.03.png]]
现在我们想要最大化 flow $s$ to $t$。我们用$f_e$表示 边$e$的 Flow

我们希望的是 每个节点都尽量 flow-in = flow-out
![[Screenshot 2025-03-05 at 21.51.10.png]]
**在Flow Network中Cycle是不会影响最终结果的**

### Ford-Fulkerson Algorithm
**在Flow Network中，该算法的核心思想是找到一个从 source 到 sink的还有可用capacity的path（称为augmented path），然后将尽可能多的flow 放在这个path上。该算法通过不断发现新的augmented path，直到最后达到maximum flow**

Residual Network Graph - indicates how much more flow is allowed in each edge in the network graph.
![[Screenshot 2025-03-05 at 23.32.20.png]]
![[Screenshot 2025-03-05 at 23.35.38.png]]







## MF2 - Max-Flow Min Cut


## MF4 - Edmonds-Karp Algorithm





















