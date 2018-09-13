---
title: 最短路径算法
prev: /7图算法/description
next: /7图算法/dfsandbfs
weight: 10
---

我们有一个有权图G，并且有一个特定的顶点s作为出发点，需要计算s到G中其他顶点之间的最短路径。

### dijkstra算法
解决单源最短路径问题的一般方法叫做dijkstra算法。这个算法是贪婪算法的最好例子。贪婪算法一般分阶段去求解一个问题，在每个阶段都把出现的当做最好的处理。

![graph2](/img/ch7/graph2.png)

### 思路
首先我们给每个顶点维护三个特征：是否已知，到出发点的距离，路径（前驱节点）。我们从出发点开始，每次选择距离出发点最近（贪婪）的节点一个未知节点，将其变为已知。并更新其他未知节点到出发点的距离。

### 伪代码
```
while True:
  v <- 未知顶点中distance最小的。
  if 没有未知顶点：
    break
  v.know = True

  for 每个和v相邻的顶点w：
    if w 未知：
      if v.dist + c和w间的距离 < w.dist:
        w.dist = v.dist + c和w间的距离
        w.path = v
```

### 实例
以上图为例，我们先建立一个初始表.这时候所有点都未知

v | 是否已知 | `$d_v$` | `$p_v$`
--- | --- | --- | ---
`$v_1$` | F | 0 | 0
`$v_2$` | F | `$\infty$` | 0
`$v_3$` | F | `$\infty$` | 0
`$v_4$` | F | `$\infty$` | 0
`$v_5$` | F | `$\infty$` | 0
`$v_6$` | F | `$\infty$` | 0
`$v_7$` | F | `$\infty$` | 0

首先我们选择V1作为出发点，从v1出发走一步可以到达v2和v4,我们更新这两个点的distance,并将它们的path设为v1.

v | 是否已知 | `$d_v$` | `$p_v$`
--- | --- | --- | ---
`$v_1$` | T | 0 | 0
`$v_2$` | F | 3 | `$v_1$`
`$v_3$` | F | `$\infty$` | 0
`$v_4$` | F | 1 | `$v_1$`
`$v_5$` | F | `$\infty$` | 0
`$v_6$` | F | `$\infty$` | 0
`$v_7$` | F | `$\infty$` | 0

然后，我们选择v4（未知点中距离出发点最近）并标记为known，从v4出发可以到达v3,v5,v6,v7。更新这几个节点的distance，并将其path设为v4

v | 是否已知 | `$d_v$` | `$p_v$`
--- | --- | --- | ---
`$v_1$` | T | 0 | 0
`$v_2$` | F | 3 | `$v_1$`
`$v_3$` | F | 3 | `$v_4$`
`$v_4$` | T | 1 | `$v_1$`
`$v_5$` | F | 3 | `$v_4$`
`$v_6$` | F | 9 | `$v_4$`
`$v_7$` | F | 5 | `$v_4$`

接下来我们选择v2（v2,v3,v5都是最近，随便选一个），v2出发可以到达v4和v5,但是并没有缩短这两个点的distance，所以我们不更新这两个点。

v | 是否已知 | `$d_v$` | `$p_v$`
--- | --- | --- | ---
`$v_1$` | T | 0 | 0
`$v_2$` | T | 3 | `$v_1$`
`$v_3$` | F | 3 | `$v_4$`
`$v_4$` | T | 1 | `$v_1$`
`$v_5$` | F | 3 | `$v_4$`
`$v_6$` | F | 9 | `$v_4$`
`$v_7$` | F | 5 | `$v_4$`

然后是v3,v3出发可以到v1和v6。原本到v1-v4-v6的距离为9，现在v1-v4-v3-v6的距离为8，所以我们更新v6

v | 是否已知 | `$d_v$` | `$p_v$`
--- | --- | --- | ---
`$v_1$` | T | 0 | 0
`$v_2$` | T | 3 | `$v_1$`
`$v_3$` | T | 3 | `$v_4$`
`$v_4$` | T | 1 | `$v_1$`
`$v_5$` | F | 3 | `$v_4$`
`$v_6$` | F | 8 | `$v_3$`
`$v_7$` | F | 5 | `$v_4$`

然后是v5,没啥可更新的

v | 是否已知 | `$d_v$` | `$p_v$`
--- | --- | --- | ---
`$v_1$` | T | 0 | 0
`$v_2$` | T | 3 | `$v_1$`
`$v_3$` | T | 3 | `$v_4$`
`$v_4$` | T | 1 | `$v_1$`
`$v_5$` | T | 3 | `$v_4$`
`$v_6$` | F | 8 | `$v_3$`
`$v_7$` | F | 5 | `$v_4$`

然后设置v7为known,v1-v4-v7-v6距离只有6，更新v6的distance和path。

v | 是否已知 | `$d_v$` | `$p_v$`
--- | --- | --- | ---
`$v_1$` | T | 0 | 0
`$v_2$` | T | 3 | `$v_1$`
`$v_3$` | T | 3 | `$v_4$`
`$v_4$` | T | 1 | `$v_1$`
`$v_5$` | T | 3 | `$v_4$`
`$v_6$` | F | 6 | `$v_7$`
`$v_7$` | T | 5 | `$v_4$`

最后我们得到我们的结果

v | 是否已知 | `$d_v$` | `$p_v$`
--- | --- | --- | ---
`$v_1$` | T | 0 | 0
`$v_2$` | T | 3 | `$v_1$`
`$v_3$` | T | 3 | `$v_4$`
`$v_4$` | T | 1 | `$v_1$`
`$v_5$` | T | 3 | `$v_4$`
`$v_6$` | T | 6 | `$v_3$`
`$v_7$` | T | 5 | `$v_4$`

### 代码实现
```python
MAX_NUM = 1000000
import copy
class Edge:
	def __init__(self, neighbour=None, distance=MAX_NUM):
		self.neighbour = neighbour
		self.distance = distance

class Vertex:
	def __init__(self,label=None):
		self.neighbours = []
		self.visited = False
		self.label =label
		self.path = None

class Graph:
	def __init__(self):
		self.vertexs = []
		self.edges = []

	def addVertex(self, label):
		vertex = Vertex(label)
		self.vertexs.append(vertex)
		return vertex

	def addEdge(self, source, dest, weight):
		edge = Edge(dest, weight)
		source.neighbours.append(edge)

def dijkstra(graph, source):
	for vertex in graph.vertexs:
		vertex.dist = MAX_NUM
		vertex.known = False
	source.dist = 0
	while True:
		#smallest unknown distance vertex
		min_dist = MAX_NUM
		v = Vertex()
		for node in graph.vertexs:
			if node.known == False and node.dist < min_dist:
				min_dist = node.dist
				v = node

		if not v.label:
			break
		v.known = True

		for edge in v.neighbours:
			if not edge.neighbour.known:
				if v.dist + edge.distance < edge.neighbour.dist:
					edge.neighbour.dist = v.dist + edge.distance
					edge.neighbour.path = v


def print_path(v):
	if v.path:
		print_path(v.path)
	print v.label


from nose.tools import assert_equal

def test():
	graph = Graph()
	nodeA = graph.addVertex('A')
	nodeB = graph.addVertex('B')
	nodeC = graph.addVertex('C')
	nodeD = graph.addVertex('D')
	nodeE = graph.addVertex('E')
	nodeF = graph.addVertex('F')

	graph.addEdge(nodeA, nodeB, 6)
	graph.addEdge(nodeA, nodeC, 3)
	graph.addEdge(nodeB, nodeC, 2)
	graph.addEdge(nodeB, nodeD, 5)
	graph.addEdge(nodeC, nodeD, 3)
	graph.addEdge(nodeC, nodeE, 4)
	graph.addEdge(nodeD, nodeE, 2)
	graph.addEdge(nodeD, nodeF, 3)
	graph.addEdge(nodeE, nodeF, 5)

	dijkstra(graph, nodeA)

	assert_equal(nodeA.dist,0)
	assert_equal(nodeB.dist,6)
	assert_equal(nodeC.dist,3)
	assert_equal(nodeD.dist,6)
	assert_equal(nodeE.dist,7)
	assert_equal(nodeF.dist,9)

	print "success"
test()
```
