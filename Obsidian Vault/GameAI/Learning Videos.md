

## Homework 8 - Procedural Content Generation (PCG, 内容动态生成)

### Homework Videos：
- **Terrain层**：没有定义Perlin Scalar（值为0），意味着最底层是平的，没有突起的形状
- **Biomes层**：The Biomes is like a probability map. Then we apply a second PCG to generate mountains. We create a Trapezoid Function PCG with 4 elements. An example of elements is like this:
	- Element 0: 0.5 ==> (0.5, 0)
	- Element 1: 0.6 ==> (0.6, 1)
	- Element 2: 1  ==> (1, 1)
	- Element 3: 1 ==> (1, 1)
	This means everything below 0.5 will be 0. Anything above 0.6 is 1.
	So you can see a few mountain-like shapes：
	![[Screenshot 2024-04-04 at 15.45.19.png]]
- 










---
### Rule System Methods
#### 原胞自动机（Cellular Automaton）
好处：
* 容易实现
* complex behavior from small set of rules
* intuitive mapping to game space generation
* local coherence

坏处：
* 依赖于猜测和对于改动的检测
* 较难检测到edge cases
* 无法避免undesired output
* 上限太低
* global incoherrence


#### 如何自己创建一个我的世界？（Minecraft）
1. Random Noise
2. Rules-based (also called agent-based)
3. Multi-agent-based approaches

#### Terrain Features
* landscapes are not complete without various objects such as trees, bushes, rocks (decorations)
* object placement is a critical step in most PCG terrains
* will be covered in the next lecture

#### Object Placement
Using RNG for object placement can result in clumping

##### **Halton Sequence** 可以用来避免出现clumping
原理是co-prime numbers之间没有公因数。案例如下：
![[Screenshot 2024-04-02 at 16.43.10.png]]

```python
def halton_sequence(base, index):
	result = 0
	denom = 1

	while index > 0:
		denom *= base
		result += (index % base) / denom
		index = math.floor(index/base)
	return result


def halton_seq_2d(basex, basey, index):
	x = halton_sequence(basex, index)
	y = halton_sequence(basey, index)

	return x, y
```

##### Poisson Distribution也可以用来避免Clumping















---

## Homework 7 - Fuzzy Logic and Racktrack
### Fuzzy Logic
 
Fuzzy Logic也就是模糊逻辑，与有限状态机（FSM）不同的是，Fuzzy Logic中每个状态之间的转换是模糊的。例如当温度**很冷**的时候，打开暖气。很冷就是一个没有被量化、模糊的界定。这么做的意义主要是为了节省计算资源。由于本身的模糊性，导致Fuzzy Logic在学术上并没有太多的应用。但是在工业界应用广泛。

Fuzzy Logic通常分为 1) Fuzzification; 2) Fuzzy Rules; 3) De-Fuzzification这三个步骤
![[Screenshot 2024-03-22 at 12.00.17.png]]


![[Screenshot 2024-03-22 at 12.09.04.png]]

Defuzzification
常见的方法是使用Center of Gravity














---
## Homework 6 - Finite State Machine (FSM) Theory

### 6.1. Mealy & Moore
- Mealy Output = F(state, input)
- Moore Output = F(state)
![[Screenshot 2024-03-12 at 16.56.54.png]]

这次的作业里只是通过代码定义了FSM；使用Enum定义了各种State

#### 6.1.1. FSM - Changing State Safely
- 状态更改通常可以在任何地方发生，包括在特定FSM状态的UpdateState()中
- 这可能会造成麻烦的情况，即当前状态会改变状态，但会执行进一步的代码
- 如果调用EnterState()，然后在退出状态下执行进一步的代码，程序员很可能最终会因意想不到的副作用而搬起石头砸自己的脚
- 因此，实施一种机制来实施延迟状态更改可能是可取的。
- 例如，只允许状态更改请求作为UpdateState()返回值的一部分，该返回值指定:“保持当前状态”或“更改到这个新状态(这里是调用的一些参数)”。
#### 6.1.2. Inaction Problem
* 编写只做决策但自身不执行操作的FSM状态，而不是转换到执行操作的状态，这可能很诱人。
* 如果FSM在每个模拟帧上执行状态，这可能导致在采取行动之前丢失帧
* 一些FSM框架可能会指示立即(同一帧转换)以避免延迟
* 必须小心避免无限循环或每帧的任意开销。
* 通常最好实现用于确定决策的方法，而不是为此目的使用FSM状态










---
## Homework 5 - Projectile
定义：
* $p_t$ - 静止的target在时间$t$时的位置
* $p_{p_0}$ - 射弹的初始位置
* $u_{p_0}$ - 射弹最初的方向
* $s_p$ - 射弹的初始速率（speed，标量）
* $v_p$ - 射弹的初速度（velocity，矢量）
* $g_p$ - 射弹所受的重力（矢量）
* $t_c$ - 撞击发生的时间点

### 5.3. 如何解决变速移动的target？——Iterative Approach

关键步骤：
1. 假设target不动
2. 求解第1步中target的射弹截距$t_n$
3. 将$t_0$带入target的运动方程$p_{t_n}=p_{t_0}+v_{t_0}t_n+\frac{1}{2}a_t t_n^2$
4. 然后回到第2步，将target的位置替换为$p_{t_n}$
5. 重复上面四个步骤直到收敛到某个特定condition，或者多少次迭代后停止。


在5.2中提到的cosine还有其他一些用途：
* 通过iterative + cosine可以来求出最大速率
* 将初始值设置为$S_{holdback}=S_p$
* 


### 5.2. 对于恒定速度移动的target
假设target和射弹都以恒定的速度移动，但是对于射弹来说，它的速度是已知的，target速率已知。此外，3d场景中，物体受到重力影响
定义：
* $p_{t_0}$ - target在起始时间$t_0$时的位置
* $p_{p_0}$ - 射弹的初始位置
* $v_t$ - target的速度（velocity，矢量）
* $s_t$ - target的速率（speed，标量）
* $s_p$ - 射弹的速率（speed， 标量）
* $v_p$ - 射弹的速度（velocity，矢量）
* $t_c$ - 撞击发生的时间
![[Screenshot 2024-02-12 at 20.28.44.png]]
<span style="color:cyan">如图，我们要求的是射弹的速度（大小和方向），使得射弹在</span>$t_c$<span style="color:cyan">时间后能撞击target。</span>
抽象成下图，然后使用余弦公式$a^2+b^2-ab \cos \theta=c^2$；点乘公式$\mathbfit{a}\cdot\mathbfit{b} = \| \mathbfit{a} \| \| \mathbfit{b} \| \cos \theta$，$\hat{a} \cdot \hat{b}=\cos \theta$。
![[Screenshot 2024-02-12 at 20.30.00.png]]
套一下公式得到：
$(s_p^2-s_t^2)t^2+(2as_t \hat{\mathbfit{a}}\cdot \hat{\mathbfit{b}})t-a^2=0$
![[Screenshot 2024-02-12 at 20.46.37.png]]
求解一下二次方程，得到：
$$
t_c=\frac{\sqrt{-(2as_t \hat{\mathbfit{a}}\hat{\mathbfit{b}})^2+4(s_p^2-s_t^2)a^2}}{2(s_p^2-s_t^2)}
$$
最终计算出射弹的速度（矢量）为：$\mathbfit{v}_p=\frac{(\mathbfit{p}_{t_0}-\mathbfit{p}_{p_0})}{t_c}+\mathbfit{v}_t$


如果是在3d空间下，受到重力影响呢？——结果如下：
![[Screenshot 2024-02-12 at 20.53.50.png]]








### 5.1. 对于静止target的射弹
3d场景中，假设受到重力影响，但没有牵引力
定义：
* $p_t$ - 静止的target在时间$t$时的位置
* $p_{p_0}$ - 射弹的初始位置
* $u_{p_0}$ - 射弹最初的方向
* $s_p$ - 射弹的初始速率（speed，标量）
* $v_p$ - 射弹的初速度（velocity，矢量）
* $g_p$ - 射弹所受的重力（矢量）
* $t_c$ - 撞击发生的时间点











## Homework 4 - NavMesh
### 4.1. 理论思想
与Grid Lattice相比，NavMesh是一种Non-Uniform Representation of Navigable Area。Grid Lattice是通过将地图平均切分成等大的小方格来实现导航功能的。在NavMesh中，地图并不是等分的，**而是将地图切分成几个不等大的convex polygons**。

在构建多边形的时候，我们最好只考虑凸多边形（convex polygon），例如下图第三个。（前两个并不适合）
![[Screenshot 2024-02-06 at 13.15.27.png]]

### 4.2. 如何生成NavMesh
#### 4.2.1. Greedy Algorithm
* Goal: Create triangles exhaustively through scene
	1. Iterate through all obstacle/boundary points selecting point `a`
	2. Then iterate through all obstacle/boundary points selecting point `b`
	3. Iterate through all obstacle/boundary points selecting point `c`
	4. This defines a candidate triangle...


1. <span style="color:cyan">检查每个三角形的每条边，如果某条边既是三角形的边，也是某个障碍物的边，那么该条边就是valid的</span>。（但注意，如果是三角形的边与障碍物的某个边相交，则不是valid的）只要当前这条三角形的边未能被定义为valid的，就break当前loop，找下一条三角形的边
2. 这意味着我们需要三层循环：
```csharp
for (int i = 0; i < len - 2; i++) {            // 遍历三角形第一个顶点a
	for (int j = i + 1; j < len - 1; j++)  {   // 遍历三角形第二个顶点b
		for (int k = j + 1; k < len; k++) {    // 遍历三角形第三个顶点c
			
		}
	}
}
```
可以稍微提高上述代码性能的方法是通过创建一个hash-table来存储intersections，也就是当前边与障碍物哪条边之间有相交。如果之前已经判断过这一对组合的话，就直接跳过。

##### 4.2.1.1. 盲目使用Greedy创建NavMesh
Adjacency Blocking。正如上面所说，<span style="color:cyan">检查每个三角形的每条边，如果某条边既是三角形的边，也是某个障碍物的边，那么该条边就是valid的</span>。<span style="color:red; font-weight: bold">也就意味着每两个相邻且valid三角形之间都是共享一条公共边的</span>。我们看下面的例子：
![[Screenshot 2024-02-06 at 14.06.47.png]]
如果使用greedy algorithm构建，就会出现截图中第一个的情况，导致ADF和BCE两个三角形之间没有共享一条公共边。
正确的方式是第二种情况，让BC作为BCF和BCE的公共边。该如何做呢？——加一个additional test如下：
![[Screenshot 2024-02-06 at 14.08.47.png]]
另一种问题是当我们要处理瘦长的三角形的时候，由于floating point value，会导致三角形ABC（逆时针）的三个顶点的相对位置有变化（如，变成ACB）。这时候我们可以用性能更好的算法例如Delaunay Triangulation（略）
<span style="color:red">Greedy Algorithm最大的问题还是慢。复杂度为O(n^4)</span>。





#### 4.2.2. Triangulation Algorithm


#### 4.2.3. Recast and NEOGEN





---

## Homework 3 - A* Path Finding Algorithm
### 3.1. 

> [知乎] https://zhuanlan.zhihu.com/p/385733813 这里提供了非常详细的算法和算例说明
> [视频8:01] 

$$
f(n) = g(n) + h(n), \space\space
其中，\space g(n) 是从起始方格到方格n的实际代价；h(n)是从格子n到终点格子的估计代价
$$

![[Pasted image 20240131103715.png]]
- 格子1：绿点到1要行动移动一格，因此 g(1)=1，格子1到红点的直线距离为6个格子，因此 h(1)=6，所以 f(1)=1+6=7 。
- 格子2：绿点到2要行动移动一格，因此 g(2)=1，格子2到红点的直线距离为4个格子，因此 h(2)=4，所以 f(2)=1+4=5 。
- 格子3：绿点到3要对角移动，因此 g(1)= $\sqrt{2}$ ，格子1到红点的直线距离为 $\sqrt{17}$ （直角三角形斜边），因此h(3)= $\sqrt{17}$ ，所以 f(3)= $\sqrt{2}$+$\sqrt{17}$ =5.54 。
可以看出，f(n) 的值基本代表着从起点格子到格子n再到终点这一段路径的长度。由于 f(2) < f(3) < f(1)，因此我们优先应该考虑去往格子2的情况。

在上面，我们 h(n) 指的是格子与格子间的直线距离，也就是**欧几里得距离**，然而它有两个**弊端**：

- 计算过程中伴随着平方与开根号运算，并且需要使用浮点数，性能差。
- 实际过程中格子3并不能直接平滑的移动到红色格子，而需要水平+对角移动结合。即若没有障碍物，实际的 h(3) = 2 +3，而不是 17 。也就是说用欧几里得距离时， **h(n) 的值永远小于或等于格子n到终点的最短实际距离。**

因此针对上述这些问题，我们 h(n) 用的更多的是曼哈顿距离或者是对角线+直线的距离。
**欧几里得距离示意图：**
![[Pasted image 20240131103859.png]]

**曼哈顿距离：**
曼哈顿距离简单来说就是**只准水平或垂直移动**的最短距离，示意图如下：

![](https://pic3.zhimg.com/80/v2-f73ab884cdcb24a8656ae1b40ee5f622_1440w.webp)

图中从格子n到终点有三条不同的路径，但是它们的总长度确是相同的，这个长度也就是我们的曼哈顿距离，也就是两个格子水平方向的差值与竖直方向的差值和。

这样相比欧几里得距离，只需要计算加减法，并且连浮点数都不需要。但是由于我们的格子可以对角线移动，因此不考虑障碍物的话，或者障碍物在两个格子形成的包围盒内，**曼哈顿距离肯定大于或等于格子n到终点的最短实际距离。**

**对角线+直线距离**
既然我们可以对角线移动，那么我们就可以根据水平方向的差值与竖直方向的差值中较小的那个值，计算出对角线，然后再平移。示意图如下：
![](https://pic3.zhimg.com/80/v2-fe8c4fa309e2e77bd8633128a655c9ea_1440w.webp)

这样不考虑障碍物的情况下，肯定**等于格子n到终点的最短实际距离。**

但是由于计算对角线同样需要开根号以及浮点数。为了加快计算，我们可以**假设两个格子间的距离为10，然后直接认为对角线距离为14（不是根号200了）**，这样就可以避免浮点数和根号运算了。


**h(n)的影响：**
直接来看一个例子，如下图：
![](https://pic1.zhimg.com/80/v2-8caa5c655eb6417494d1e36091a691e4_1440w.webp)

图中g(1)=g(2)=20，若h(n)使用曼哈顿距离，则h(1)=h(2)=60，即f(1)=f(2)，导致我们无法判断出走1和走2哪个更好。但是若使用对角线距离，则h(1)=60，h(2)=42，f(1)>f(2)，因此我们下一步要走2。

实际上走2才是最短的路径，但是由于有障碍物，格子2到红格子的实际距离为10+14+14+10=48（右->右下->右下->下），可以发现这种情况下：曼哈顿距离>实际距离>对角线距离。

再来看一个例子：
![](https://pic3.zhimg.com/80/v2-f34196f91853315065d003111d4e1d06_1440w.webp)

若使用曼哈顿距离，f(1)=g(1)+h(1)=14+190=204，f(2)=g(2)+h(2)=74+90=184，就是说我宁可考虑格子2也不会去考虑格子1。

但是使用对角线距离，f(1)=g(1)+h(1)=14+136=150，f(2)=g(2)+h(2)=74+78=152，那就变得格子1要优先考虑。

两种h(n)得到的路径如下：
曼哈顿距离的结果：
![](https://pic2.zhimg.com/80/v2-4a3010191cf96bde40881ae7c598ef89_1440w.webp)



对角线距离的结果：
![](https://pic2.zhimg.com/80/v2-141d37a4d64adbef8f5bf7a2dde385b5_1440w.webp)

可以发现，对角线距离的结果才是最短的路径，但是它会计算更多的格子（图中两种蓝色的格子就是要计算的格子）。

总结来说：
- 如果 h(n) <= n到终点的实际距离，A*算法可以找到最短路径，但是搜索的点数多，搜索范围大，效率低。
- 如果 h(n) > n到终点的实际距离，搜索的点数少，搜索范围小，效率高，但是得到的路径并不一定是最短的。
- h(n) 越接近 n到终点的实际距离，那么A*算法越完美。（个人理解是如果用曼哈顿距离，那么只需要找到一条长度小于等于该距离的路径就算完成任务了。而使用对角线距离就要找到一条长度大于等于对角线距离且最短的路径才行。）
- 若 h(n)=0，即 f(n)=g(n)，A*算法就变为了Dijkstra算法（Dijstra算法会毫无方向的向四周搜索）。
- 若 h(n) 远远大于 g(n) ，那么 f(n) 的值就主要取决于 h(n)，A*算法就演变成了BFS算法。

**因此在启发式搜索中，估价函数是十分重要的，采用了不同的估价可以有不同的效果。**

  

**具体寻路过程：**
一些基本的概念介绍完后，我们来看看怎么A*算法的具体寻路是怎么样的，基本上就是说，哪些格子我们要去估价，然后这些格子按什么顺序去估价。

我们从最简单的场景入手来理解，如下图：
![](https://pic2.zhimg.com/80/v2-a71e0828cc057708b37a424c280ed851_1440w.webp)


**第一步：**因为我们的绿点可以往周边8个格子移动，那么我们就要用估价函数计算它周边格子的值，来看看往哪走比较好，得到结果如下（使用对角线距离估价）：
![](https://pic4.zhimg.com/80/v2-20cf9ec7e8c9b9e85eec9cc6bf916973_1440w.webp)
因为我们是通过绿色格子计算得到这8个格子的，因此它们都指向绿色格子（格子中的箭头），或者称绿色格子是它们的parent。


**第二步：**我们找到第一步8个格子中f(n)值最小的格子（格子0），然后再计算它周边格子的f(n)，如下图：
![](https://pic4.zhimg.com/80/v2-50df65721da04e444ef811e2b3a2f447_1440w.webp)
此时格子0周边格子的g(n)应该是g(0)的值加上自己到格子0的距离。例如格子1此时的g(1)应该为g(0)+14=24，即2-0-1的顺序。但是由于格子1在第一步已经算过了，当时g(1)=10，2-1的顺序。这里我们**要用较小的那个值，因为g值小，说明路径短**。格子3,4,5同理。而格子6之前没有计算过，因此f(6)=g(6)+h(6)=(g(0)+14)+h(h)，顺序2-0-6。

格子7,8由于是障碍物，直接不管就行。格子2由于之前已经计算过它周边了，没有再计算它的意义了，也不用管。

**第三步：**我们从剩下的8个深蓝色的格子中再找出f(n)最小的格子，由于有3个等于58的格子，我们随便取一个计算它周边的格子，如下图：
![](https://pic2.zhimg.com/80/v2-8f0dce000e785f2d8c4ab23350161975_1440w.webp)
这里可以发现，格子1的g值并不是最小的，但是不要紧，当我们后面计算到格子2时，会更新格子1的g值（前面说了会用较小的g值），并且箭头指向格子2。

**第四步...第n步**：一直找出深蓝色格子中的f(n)最小的格子，然后计算周边格子的估价值。

**最后一步：**当我们发现某个格子（格子1）周边有个格子是终点格子时，就说明我们找到了到终点的最短路径。
![](https://pic3.zhimg.com/80/v2-86a4bfc559428bfa897a8e41a08e32b6_1440w.webp)


我们只需要根据格子1的箭头指向一直往前就可以得到完整的路径：
![](https://pic4.zhimg.com/80/v2-662070669b3052453f41972ce656219b_1440w.webp)

---

**A\*代码**
```csharp
public class AStar {
    static int FACTOR = 10;//水平竖直相邻格子的距离
    static int FACTOR_DIAGONAL = 14;//对角线相邻格子的距离

    bool m_isInit = false;
    public bool isInit => m_isInit;

    UIGridController[,] m_map;//地图数据
    Int2 m_mapSize;
    Int2 m_player, m_destination;//起始点和结束点坐标
    EvaluationFunctionType m_evaluationFunctionType;//估价方式

    Dictionary<Int2, Node> m_openDic = new Dictionary<Int2, Node>();//准备处理的网格
    Dictionary<Int2, Node> m_closeDic = new Dictionary<Int2, Node>();//完成处理的网格

    Node m_destinationNode;

    public void Init(UIGridController[,] map, Int2 mapSize, Int2 player, Int2 destination, EvaluationFunctionType type = EvaluationFunctionType.Diagonal) {
        m_map = map;
        m_mapSize = mapSize;
        m_player = player;
        m_destination = destination;
        m_evaluationFunctionType = type;

        m_openDic.Clear();
        m_closeDic.Clear();

        m_destinationNode = null;

        //将起始点加入open中
        AddNodeInOpenQueue(new Node(m_player, null, 0, 0));
        m_isInit = true;
    }

    //计算寻路
    public IEnumerator Start() {
        while(m_openDic.Count > 0 && m_destinationNode == null) {
            //按照f的值升序排列
            m_openDic = m_openDic.OrderBy(kv => kv.Value.f).ToDictionary(p => p.Key, o => o.Value);
            //提取排序后的第一个节点
            Node node = m_openDic.First().Value;
            //因为使用的不是Queue，因此要从open中手动删除该节点
            m_openDic.Remove(node.position);
            //处理该节点相邻的节点
            OperateNeighborNode(node);
            //处理完后将该节点加入close中
            AddNodeInCloseDic(node);
            yield return null;
        }
        if(m_destinationNode == null)
            Debug.LogError("找不到可用路径");
        else
            ShowPath(m_destinationNode);
    }

    //处理相邻的节点
    void OperateNeighborNode(Node node) {
        for(int i = -1; i < 2; i++) {
            for(int j = -1; j < 2; j++) {
                if(i == 0 && j == 0)
                    continue;
                Int2 pos = new Int2(node.position.x + i, node.position.y + j);
                //超出地图范围
                if(pos.x < 0 || pos.x >= m_mapSize.x || pos.y < 0 || pos.y >= m_mapSize.y)
                    continue;
                //已经处理过的节点
                if(m_closeDic.ContainsKey(pos))
                    continue;
                //障碍物节点
                if(m_map[pos.x, pos.y].state == GridState.Obstacle)
                    continue;
                //将相邻节点加入open中
                if(i == 0 || j == 0)
                    AddNeighborNodeInQueue(node, pos, FACTOR);
                else
                    AddNeighborNodeInQueue(node, pos, FACTOR_DIAGONAL);
            }
        }
    }

    //将节点加入到open中
    void AddNeighborNodeInQueue(Node parentNode, Int2 position, int g) {
        //当前节点的实际距离g等于上个节点的实际距离加上自己到上个节点的实际距离
        int nodeG = parentNode.g + g;
        //如果该位置的节点已经在open中
        if(m_openDic.ContainsKey(position)) {
            //比较实际距离g的值，用更小的值替换
            if(nodeG < m_openDic[position].g) {
                m_openDic[position].g = nodeG;
                m_openDic[position].parent = parentNode;
                ShowOrUpdateAStarHint(m_openDic[position]);
            }
        }
        else {
            //生成新的节点并加入到open中
            Node node = new Node(position, parentNode, nodeG, GetH(position));
            //如果周边有一个是终点，那么说明已经找到了。
            if(position == m_destination)
                m_destinationNode = node;
            else
                AddNodeInOpenQueue(node);
        }
    }

    //加入open中，并更新网格状态
    void AddNodeInOpenQueue(Node node) {
        m_openDic[node.position] = node;
        ShowOrUpdateAStarHint(node);
    }

    void ShowOrUpdateAStarHint(Node node) {
        m_map[node.position.x, node.position.y].ShowOrUpdateAStarHint(node.g, node.h, node.f,
            node.parent == null ? Vector2.zero : new Vector2(node.parent.position.x - node.position.x, node.parent.position.y - node.position.y));
    }

    //加入close中，并更新网格状态
    void AddNodeInCloseDic(Node node) {
        m_closeDic.Add(node.position, node);
        m_map[node.position.x, node.position.y].ChangeInOpenStateToInClose();
    }

    //寻路完成，显示路径
    void ShowPath(Node node) {
        while(node != null) {
            m_map[node.position.x, node.position.y].ChangeToPathState();
            node = node.parent;
        }
    }

    //获取估价距离
    int GetH(Int2 position) {
        if(m_evaluationFunctionType == EvaluationFunctionType.Manhattan)
            return GetManhattanDistance(position);
        else if(m_evaluationFunctionType == EvaluationFunctionType.Diagonal)
            return GetDiagonalDistance(position);
        else
            return Mathf.CeilToInt(GetEuclideanDistance(position));
    }

    //获取曼哈顿距离
    int GetDiagonalDistance(Int2 position) {
        int x = Mathf.Abs(m_destination.x - position.x);
        int y = Mathf.Abs(m_destination.y - position.y);
        int min = Mathf.Min(x, y);
        return min * FACTOR_DIAGONAL + Mathf.Abs(x - y) * FACTOR;
    }

    //获取对角线距离
    int GetManhattanDistance(Int2 position) {
        return Mathf.Abs(m_destination.x - position.x) * FACTOR + Mathf.Abs(m_destination.y - position.y) * FACTOR;
    }

    //获取欧几里得距离,测试下来并不合适
    float GetEuclideanDistance(Int2 position) {
        return Mathf.Sqrt(Mathf.Pow((m_destination.x - position.x) * FACTOR, 2) + Mathf.Pow((m_destination.y - position.y) * FACTOR, 2));
    }

    public void Clear() {
        foreach(var pos in m_openDic.Keys) {
            m_map[pos.x, pos.y].ClearAStarHint();
        }
        m_openDic.Clear();

        foreach(var pos in m_closeDic.Keys) {
            m_map[pos.x, pos.y].ClearAStarHint();
        }
        m_closeDic.Clear();

        m_destinationNode = null;

        m_isInit = false;
    }
}
```


#### 补充

最近回头看寻路相关的东西，看自己这个demo，发现一个很呆逼的情况，如下图：

![动图封面](https://pic3.zhimg.com/v2-638a13e9502db9b13d96b4fc37267066_b.jpg)

很呆逼的在往上计算一些明显没用的格子，造成浪费

如此一个简单的场景，最终出现了不少不必要访问的格子：
![](https://pic1.zhimg.com/80/v2-647f5bbe64d72502cfd92c4cd8dd386c_1440w.webp)
这是因为之前的代码只是根据F的值做了个升序处理，当有多个最小的F值格子时，一个选择的失误就造成了多次可能无效的计算。

![](https://pic1.zhimg.com/80/v2-090a1644a7ce02cc85483a0521fac578_1440w.webp)
那么当F值相同时，如何更智能的选择格子呢？我们可以优先选择H值更小的那个格子。因为H值越小，代表它越接近目标，在当前格子到目标之间没有障碍物的情况下，这一定是最优的选择（例如上面例子）。

修改代码如下：
```csharp
//先按照f的值升序排列，当f值相等时再按照h的值升序排列
m_openDic = m_openDic.OrderBy(kv => kv.Value.f).ThenBy(kv => kv.Value.h).ToDictionary(p => p.Key, o => o.Value);
```

修改后的寻路过程：

![动图](https://pic4.zhimg.com/v2-17bf7db33fb216ae0ec2ee3c4f1dfc33_b.webp)
不再呆逼的往上去找了

  

其他的一些优化：

当我们地图被某些障碍物分割，只留了一两个可通过的小口时，可以将寻路过程拆解开来，例如下图：
![](https://pic3.zhimg.com/80/v2-bfae6164001ae88ebf2fdc62232b1e82_1440w.webp)
障碍物左侧做了很多没有意义的格子计算

可以将起点到终点的寻路过程拆分为起点到A点，A点到终点的两段寻路过程，来减少不必要的计算。像皇室战争这样的地图就可以使用这样的方式：
![](https://pic4.zhimg.com/80/v2-cc0d2ba19e532b8aef45d68b804e505f_1440w.webp)


















## Homework 2 - Path Network Navigation

### 2.1. Basic Concepts
**Path Network** - 就是一组路径节点和边（path nodes and edges），他们的存在就是为了促进agent对于障碍物的躲避

与 grid topology不同的是，Path Network不要求agent每时每刻都要在某一个path 或者edge上（如下图），它可以在任何地方
![[Screenshot 2024-01-23 at 11.44.46.png]]
所以当agent要从A -> B点，只需要找到agent到它附近最近的边或者点，然后按照已有的Path和Node移动即可


🤔什么时候A -> B点之间是存在一个可行的边呢？
当满足以下三个条件时
1. 两个节点之间没有障碍物或者边界墙
2. 两节点之间的边 有充足的宽度能让agent通过，而不与任何障碍物相撞
3. 其中一个节点在地图边界内，但在障碍物之外
我们说Node A和B之间存在一个可行的边
（此外要注意，这里的边都是双向的。且A -> A不算做一条边）

### 2.2. Path network movement
该算法中的agent主要有两种移动方式，一种是local；一种是remote。
* local movement说的是在agent的视线之内进行移动，它的决策是基于agent（例如可以使用raycast）
* remote movement是在agent 视线之外进行移动。在remote movement的过程中，agent可以是On the path也可以是Off the path
	* 当Off the path的时候，agent就要依赖于local movement
	* 如果remote movement不可行（也就是不存在到达终点的路径。通常是由于会移动的障碍物），则需要重新规划路径

#### 算法设计实现
从上面的描述不难看出，该算法的可靠性很大程度上取决于local movement，所以local movement必须robust
下面的例子：
假设有一个agent和一个map，agent想要做一个remote movement。此时它需要找到它附近不被遮挡的node。所以使用了raycast，发现A被遮挡，G没有。所以决定移动到G点
![[Screenshot 2024-01-23 at 13.07.13.png]]

再假设我们有另一个agent在右下角，虽然它能看到两个没有遮挡的node，但是N点更近，所以率先移动到N点
![[Screenshot 2024-01-23 at 13.09.27.png]]

最后，如果蓝色agent要移动到黑色agent，那就沿着绿线移动
![[Screenshot 2024-01-23 at 13.11.59.png]]
这样移动的缺点就是，在真正的游戏中，最后蓝色agent还需要拐一下。且从D到L也有些绕远。


下面这种情况中，由于agent看不到任何节点，所以需要采用一些特殊手段。
1. 添加waypoint（之后会提到）
2. 采用local movement中 沿着墙体移动的策略
3. 创建更多的nodes，当然这样一来算法会更贵。
4. teleport agent到合适的地方
![[Screenshot 2024-01-23 at 14.23.29.png]]

#### How to Build a Path Network?
##### Manual Placement
* Tedious, but can give good results
* Can use manual node placement, but automatic edge forming


##### Automatic Placement
* Quick, but can be inefficient and temperamental

###### Flood Fill的步骤
Flood Fill是Automatic Placement用来搭建Path network的一种方法，步骤如下：
1. 初始化seed
2. 然后uniformly的添加seeds
3. 设计者之后可以手动move, delete, add seeds
4. 确保 所有nodes 和 edges都至少与agent的collider 半径一样远
5. Flood Fill可以和grid lattice一样dense，但也可以不那么dense从而节省内存空间
![[Screenshot 2024-01-23 at 14.43.01.png]]

###### Points of Visibility (POV)
另一种Automatic Placement的方法是points of visibility。它的原理是：
1. 通过convex angle obstacle（凸角障碍）本身的特性来创建 自然拐点（例如途中墙角凸出处）。从而创建有效的路径
2. 最优路径总是在凸顶点处有拐点
3. 这些点是路径节点的良好候选点
![[Screenshot 2024-01-23 at 14.55.14.png]]
需要注意的是：
1. 现实世界的地图需要简化，因为有太多的点不实用
2. 拐点必须偏移一段距离，以便为代理留出空间
	1. 沿角的平分线偏移
	2. 是否应该比agent半径更远，以允许灵活性/缓冲
❌缺点是：
1. nodes和edges可能会以 “排列组合”级 的速度增长
2. 它最终可能会掉进一个兔子洞，添加越来越多的自动放置功能，而这些功能永远不会起作用
3. 它通常需要大量的手动调整，抵消了好处
![[Screenshot 2024-01-23 at 15.14.04.png]]
* 凹角也可以用来更好地覆盖可航行空间
* 与之前的凸角偏移类似，但您还需要在角平分线上找到一个点，该点与凸角的两条线段之一重合的直线至少有agent的半径距离
	* 添加一些额外的补偿，从而agent不会卡住
* 凹角不是路径拐点，所以不是很有用
* 最好在感兴趣的点添加额外的节点
![[Screenshot 2024-01-23 at 16.36.54.png]]

###### Building a Path network from candidate nodes
* 必须评估障碍物是否侵入了由节点A和B之间的边定义的走廊
* 走廊大小要根据agent 的半径来定义，从而确保agent能够在走廊上移动
* 组合使用point-distance-to-line-segment 检测和 line-seg-line-seg-intersection 检测
* 如果走廊上没有障碍物，则将边 添加到graph上

下面是一个案例【[40:00](https://mediaspace.gatech.edu/media/2021+Game+AI+-+Path+Planning+-+Path+Networks/1_e8ttuyjc)】：
* 上面浅蓝色区域是valid edge，agent是蓝色圆形。start和end point是最上面的两个黄点
* 两个大的黄色矩形障碍物，每个都有四个concave 凸点。我们可以从每个凸点拉一个到黄线的 垂线（绿线）。看每个垂线的距离是否都大于agent的半径（如果不大于则说明障碍物会阻挡这条路径）。此外还需要检测从start point 和end point到每个地图边界的距离（绿线）是否大于agent 半径
* 此外还有一种情况是障碍物某个顶点刚好在淡蓝色区域内（说明该顶点到agent的距离小于agent半径），但很明显障碍物还是遮挡了agent的路径。
* Parallel test - 
![[Screenshot 2024-01-23 at 16.44.49.png]]


###### Breadcrumbs/Dynamic Path Network
【[44:00](https://mediaspace.gatech.edu/media/2021+Game+AI+-+Path+Planning+-+Path+Networks/1_e8ttuyjc)】
* 是动态构建的
* agents在重要地点“投放”航路点 (waypoint)
* 从HOME位置的waypoint作为图的第一个节点开始
* 每次Think()更新，检查是否从当前位置可以清晰地光线投射到最后一个路径点 (lastWaypoint)。
	* 如果是，那么存储lastSafePos变量
	* 如果没有，将lastSafePos添加到图中作为一个新节点并连接到lastWaypoint
		* 设置lastWaypoint为lastSafePos
* 附加增量更新过程形成其他边
	* 空间划分，如cell-space/bin-space，以跟踪附近的航路点 (waypoint)

**该动态Path Network的应用：**
* 在程序生成的世界中可能有用，因为预先编写或烘烤路径规划是不切实际的
* 潜在地避免了创建网络的作者负担
**缺点：**
* 失控的航路点形成会消耗内存
* 清理过程可能会变得昂贵

**Addressing Path Quality with Path Network**
* 可以利用String Pulling算法，但需要与世界几何(或者是物理colliders)相对撞。
* 或者agent可以使用Greedy Path Following方法
	* 利用agent只能在路径上或路径外这一事实

###### Path Network: Greedy Steering
* 通过智能体steering（转向）或者 提前跳过路径中它能看到的最远节点来改进路径
	* 使用raycast，但要小心pits !
* This may clip a corner
	* 使用point-dist-to-line-seg 进行测试
	* ![[Screenshot 2024-01-23 at 17.27.37.png]]
* 你也可以通过迭代细分（iterative subdividing）来查看edges。但这样做更贵

**使用Greedy Steering 算法的问题：**
* agent steerting (agent转向) 行为最终不是解决Path Network Quality Problems(路径网络质量问题)的完美方案
* raycasting 仍然会错过一些东西 且很容易变得昂贵。特别是在可变高度地形和不同地形类型的情况下
* Agent可能会走捷径，但最终会在缓慢地形中行走，而不是在快速地形的路径上行走(例如，泥泞vs道路)。
![[Screenshot 2024-01-23 at 17.31.11.png]]


### 2.3. Path Network的优劣
**优点：**
1. 空间离散化可以非常小
2. 它不需要agent一直在其中一个路径节点上(不像grid lattice)
3. 它在local 和 remote navigation之间切换
4. 它可以很好地与连续运动的agent一起工作
5. 这对于FPS和RPGs来说都很好
6. 节点可以包含有用的元数据(例如，掩体位置等)。
**缺点：**
1. 有效的代理位置可能无法看到网络中的节点(无法切换到remote movement)
2. 它的路径形状参差不齐
3. 它有动态和起伏的地形问题
4. 网络的存储和复杂性，以获得良好的覆盖和良好的路径形状(特别是自生成)
5. agent离开网络路径可能是危险的(卡住等)。
6. 潜在地将大量的实时计算压力放在代理的实现上，而不是预处理离散化上

#### Variable Size Path Network Corridor
* 改变corridor大小的Path Network
* 每个节点都有自己的半径，在它周围定义一个清晰的区域，并且连接的节点沿着一个区域(大致是一个圆锥)的连接边缘也是清晰的
* 它可以允许安全有效的局部移动(在凸区域)
	* Agent可以通过在区域内移动来避开动态障碍物
	* 机会主义行为，如战利品收集
	* 高效拉绳(走廊内)
* 简洁的走廊定义(2个矢量和2个浮点数)
* 修改点距线距离，以有效地测试点的封闭性
	* leverage line vector equal to *t* scalar to lerp between the radiuses
* 与不同半径的多个代理一起工作，通过代理半径减少每个节点半径
**缺点:**
* 具有挑战性的优化问题，自动最大化整个走廊面积
* 仍然不能完全代表可航行空间
* 不一定要在障碍物拐点处放置节点(但可能在半径范围内)
![[Screenshot 2024-01-23 at 17.46.16.png]]



---
## Basic Agent Movement ([ref](https://mediaspace.gatech.edu/media/Game+AI+-+Basic+Agent+Movement/1_e3dulou9))

1. Continuous Simulation
	1. Frame-based
	2. Closed-loop where player is part of the overall system
	3. 动画需要至少10 frame/second （强烈建议至少30帧/秒）
2. Continuous Positioning
3. Time Dependency
	1. 通常来说游戏中的帧率都是不恒定的，这对于agent的移动会有影响
		1. `newPos = oldPos + ConstantVelOffset`
		2. `newPos = oldPos + vel * deltaT` (better)
	2. 


```c#
//STUDENT CODE HERE

Vector2Int convertedOrigin = ConvertToInt(canvasOrigin);

int convertedWidth = ConvertToInt(canvasWidth);

int convertedHeight = ConvertToInt(canvasHeight);

  
  

pathEdges = new List<List<int>>(pathNodes.Count);

// for each node in pathnodes

for (int i = 0; i < pathNodes.Count; ++i)

{

List<int> pathEdge = new List<int>();

int radiusOffset = ConvertToInt(agentRadius);

Vector2Int coordsAdjA = ConvertToInt(pathNodes[i]) - convertedOrigin;

Vector2Int rightBoundA = coordsAdjA + new Vector2Int(radiusOffset, 0);

Vector2Int leftBoundA = coordsAdjA + new Vector2Int(-radiusOffset, 0);

Vector2Int upperBoundA = coordsAdjA + new Vector2Int(0, radiusOffset);

Vector2Int lowerBoundA = coordsAdjA + new Vector2Int(0, -radiusOffset);

// skip node check if the node is outside of any boundaries

if (rightBoundA.x >= convertedWidth || leftBoundA.x <= 0 || upperBoundA.y >= convertedHeight || lowerBoundA.y <= 0)

{

pathEdges.Add(pathEdge);

continue;

}

// compare node at i to every other node aside from itself

for (int j = 0; j < pathNodes.Count; ++j)

{

if (pathNodes[i] == pathNodes[j])

{

continue;

}

  

// for each obstacle, check if obstacle is blocking the path between

// either the left or right sides of the nodes

Vector2Int upperA = coordsAdjA + new Vector2Int(0, radiusOffset);

Vector2Int lowerA = coordsAdjA + new Vector2Int(0, -radiusOffset);

  

Vector2Int nodeAUpperRight = coordsAdjA + new Vector2Int(radiusOffset, radiusOffset);

Vector2Int nodeAUpperLeft = coordsAdjA + new Vector2Int(-radiusOffset, radiusOffset);

Vector2Int nodeALowerRight = coordsAdjA + new Vector2Int(-radiusOffset, radiusOffset);

Vector2Int nodeALowerLeft = coordsAdjA + new Vector2Int(-radiusOffset, -radiusOffset);

  

Vector2Int coordsAdjB = ConvertToInt(pathNodes[j]) - convertedOrigin;

Vector2Int upperB = coordsAdjB + new Vector2Int(0, radiusOffset);

Vector2Int lowerB = coordsAdjB + new Vector2Int(0, -radiusOffset);

Vector2Int nodeBUpperRight = coordsAdjB + new Vector2Int(radiusOffset, radiusOffset);

Vector2Int nodeBUpperLeft = coordsAdjB + new Vector2Int(-radiusOffset, radiusOffset);

Vector2Int nodeBLowerRight = coordsAdjB + new Vector2Int(-radiusOffset, radiusOffset);

Vector2Int nodeBLowerLeft = coordsAdjB + new Vector2Int(-radiusOffset, -radiusOffset);

  

Vector2Int rightBoundB = coordsAdjB + new Vector2Int(radiusOffset, 0);

Vector2Int leftBoundB = coordsAdjB + new Vector2Int(-radiusOffset, 0);

Vector2Int upperBoundB = coordsAdjB + new Vector2Int(0, radiusOffset);

Vector2Int lowerBoundB = coordsAdjB + new Vector2Int(0, -radiusOffset);

  

if (rightBoundB.x >= convertedWidth || leftBoundB.x <= 0 || upperBoundB.y >= convertedHeight || lowerBoundB.y <= 0)

{

continue;

}

  

bool invalidPath = false;

// also check if any part of the node is inside of the obstacle

for (int k = 0; k < obstacles.Count; k++)

{

// otherwise, a path between node i and j should exist

Vector2Int[] points = obstacles[k].getIntegerPoints();

Vector2Int[] pointsAdj = new Vector2Int[points.Length];

  

// for each point in the polygon, generate adjusted points with offset added

for (int l = 0; l < points.Length; l++)

{

Vector2Int adjustedL = points[l] - convertedOrigin;

pointsAdj.SetValue(adjustedL, l);

  

// check if any point in obstacle has a distance between two lines is less than diameter

// of the agent

float d1 = DistanceToLineSegment(adjustedL, upperA, upperB);

float d2 = DistanceToLineSegment(adjustedL, lowerA, lowerB);

  

if (d1 + d2 <= ConvertToInt(agentRadius) * 2)

{

invalidPath = true;

break;

}

  

for (int m = 0; m < points.Length; m++)

{

Vector2Int adjustedM = points[m] - convertedOrigin;

if (adjustedL == adjustedM)

{

continue;

}

// check intersection of line created by points L M

if (Intersects(adjustedL, adjustedM, coordsAdjA, coordsAdjB))

{

invalidPath = true;

break;

}

}

}

  

// if obstacle is blocking, break out of obstacle loop

if (invalidPath)

{

break;

}

// check if points of node A are in obstacle

if (IsPointInPolygon(pointsAdj, nodeAUpperRight) ||

IsPointInPolygon(pointsAdj, nodeAUpperLeft) ||

IsPointInPolygon(pointsAdj, nodeALowerRight) ||

IsPointInPolygon(pointsAdj, nodeALowerLeft))

{

// no point in checking B or other obstacles; break.

invalidPath = true;

break;

}

// check if points of node B are in obstacle

if (IsPointInPolygon(pointsAdj, nodeBUpperRight) ||

IsPointInPolygon(pointsAdj, nodeBUpperLeft) ||

IsPointInPolygon(pointsAdj, nodeBLowerRight) ||

IsPointInPolygon(pointsAdj, nodeBLowerLeft))

{

// no point in checking other obstacles; break.

invalidPath = true;

break;

}

  

if (invalidPath)

{

break;

}

}

  

if (invalidPath)

{

continue;

}

else

{

// by this point if invalidPath is still false, then path is valid.

pathEdge.Add(j);

}

}

  

// add the pathEdge set to pathEdges

pathEdges.Add(pathEdge);

}

  

// END STUDENT CODE
```













