

## 常见术语

### Centrality
* **Degree Centrality**: 度中心性是最容易计算的一种。<span style="background:#fff88f">一个节点的度中心性就是它的度——它拥有的边的数量。度越高，节点越中心。这可能是一个有效的度量，因为通过其他度量，许多具有高度的节点也具有高中心性</span>。
* **Betweenness Centrality**：中间性中心性测量一个顶点位于其他顶点之间的路径上的程度。<span style="background:#fff88f">具有高中间性的顶点可以通过控制其他顶点之间的信息传递而在网络中产生相当大的影响</span>。
* **Closeness Centrality**: 接近中心性是一种检测节点的方法，它能够非常有效地通过图传播信息。节点的<span style="background:#fff88f">接近中心性度量它到所有其他节点的平均距离(逆距离)。接近度得分高的节点到所有其他节点的距离最短</span>。
* **Harmonic Centrality**: 网络测量谐波中心性测量网络中节点到其他节点的“平均”距离。这<span style="background:#fff88f">可以用来在网络中找到可以快速与其他节点通信的节点</span>。
* **Eigenvector Centrality**: 特征向量中心性是一种<span style="background:#fff88f">度量节点传递影响</span>的算法。来自高分节点的关系比来自低分节点的连接对节点得分的贡献更大。特征向量得分高意味着一个节点与许多本身得分高的节点相连。
* **PageRank Centrality**: PageRank中心性是特征中心性的一种变体，<span style="background:#fff88f">用于对网页内容进行排名，使用页面之间的超链接作为重要性的衡量标准</span>。不过，它可以用于任何类型的网络。
* **Percolation Centrality**: 节点的渗透中心性，在给定时间，被定义为<span style="background:#fff88f">通过该节点的“渗透路径”的比例</span>。该度量基于节点的拓扑连通性以及它们的渗透状态来量化节点的相对影响。



### Community Detection
* **Modularity**: 模块化是对网络或图形结构的一种度量，它衡量了网络划分为模块(也称为组 *component*、集群 *cluster* 或社区 *community* )的强度。<font color="#92d050">模块化程度高的网络，模块内的节点之间连接密集，而不同模块间的节点之间连接稀疏</font>。模块化常用于网络群落结构检测的优化方法中。生物网络，包括动物的大脑，表现出高度的模块化。然而，模块化最大化在统计上并不一致，并且在其自己的零模型(即完全随机图)中发现社区，因此它不能用于在经验网络中找到统计上显著的社区结构。此外，已经表明，模块化受到分辨率限制，因此，它无法检测小社区。

















