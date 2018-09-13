---
title: 图的表示
prev: /7图算法
next: /7图算法/shortestpath
weight: 5
---

图的表示有两种，邻接矩阵和邻接表。

![graph](/img/ch7/graph1.png)

邻接矩阵 | 1 | 2 | 3 | 4 | 5
--- | --- | --- | --- | --- | ---
1 | 0 | 3 | 0 | 0 | 0
2 | 0 | 0 | 0 | 11 | 0
3 | 18 | 0 | 0 | 5 | 6
4 | 0 | 0 | 0 | 0 | 0
5 | 0 | 0 | 0 | 0 | 0

邻接表 | dest
--- | ---
1 | 2
2 | 4
3 | 1,4,5
4 |
5 |

### 邻接矩阵

```python
indexCounter = 0

class Graph:

    def __init__(self, matrix=[]):
        self.matrix = matrix

    def createVertex(self, data):
        size = len(self.matrix)
        for row in self.matrix:
            row.append(0)
        self.matrix.append([0]*(size+1))
        return Vertex(data)

    def connect(self, source, dest, weight):
        self.matrix[source.index][dest.index] = weight

    def weightBetween(self, source, dest):
        return self.matrix[source.index][dest.index]

class Vertex:
    def __init__(self, data):
        global indexCounter
        self.data = data
        self.index = indexCounter
        indexCounter += 1


graph = Graph()
v1 = graph.createVertex(1)
v2 = graph.createVertex(2)
v3 = graph.createVertex(3)
v4 = graph.createVertex(4)
v5 = graph.createVertex(5)
graph.connect(v1, v2, 1.0)
graph.connect(v4, v5, 3.0)
print graph.matrix
```

### 邻接表

```python
class Edge:
    def __init__(self, source=None, dest=None, weight=0):
        self.source = source
        self.dest = dest
        self.weight = weight

class Vertex:
    def __init__(self, data):
        self.data = data
        self.edges = []

    def connectTo(self,dest,weight):
        self.edges.append(Edge(source=self,dest=dest,weight=weight))

    def connectBetween(self, dest, weight):
        self.connectTo(dest, weight)
        dest.connectTo(self, weight)

    def isConnectedTo(self, dest):
        for e in self.edges:
            if e.dest == dest:
                return True
        return False
```

### 图的拓扑排序

拓扑排序是对有向无环图的顶点的一种排序。如果存在一条从`$v_i$`到`$v_j$`的路径，那么在排序中`v_j就出现在v_i的后面`。据此我们也可以得出，如果一个图有环，那么拓扑排序就不可能存在。求拓扑排序的算法是先找出任意一个没有入边的顶点，然后删除该点和从它发出的所有边。不断进行这一步骤直到取出所有顶点。
