---
title: 图的遍历
prev: /7图算法/shortestpath
next: /8贪心算法
weight: 15
toc: true
---

 图的遍历主要有两种算法，分别是广度优先(bfs)和深度优先(dfs)。

## BFS

 bfs算法可以使用队列来实现。简单来说，我们从某一个点开始，先将其所有邻居节点加入队列，并标注为visited,然后依次出队，并将出队的节点的所有邻居中还没有被经过的节点入队，直到队列为空（所有节点都已经被经过）

 ```python
 #coding:utf-8
class Edge:
    def __init__(self, neighbour=None):
        self.neighbour = neighbour

class Vertex:
    def __init__(self,label):
        self.neighbours = []
        self.visited = False
        self.label =label

class Graph:
    def __init__(self):
        self.vertexs = []
        self.edges = []

    def addVertex(self, label):
        vertex = Vertex(label)
        self.vertexs.append(vertex)
        return vertex

    def addEdge(self, source, dest):
        edge = Edge(dest)
        source.neighbours.append(edge)


class Queue:
    def __init__(self, datas=[]):
        self.datas = datas
    def __len__(self):
        return len(self.datas)
    def isEmpty(self):
        return len(self) == 0
    def enqueue(self,data):
        self.datas.append(data)
    def dequeue(self):
        return self.datas.pop(0) if not self.isEmpty() else None
    def peek(self):
        return self.datas[0] if not self.isEmpty() else None


def BFS(graph, source):
    queue = Queue()

    #初始点入队
    queue.enqueue(source)  
    result = [source.label]
    source.visited = True
    #队列非空时循环
    while not queue.isEmpty():
        #节点出队
        node = queue.dequeue()
        for edge in node.neighbours:
            #找到该节点的所有邻居，如果该邻居还没有被经过，怎将其入队
            node = edge.neighbour
            if not node.visited:
                queue.enqueue(node)
                node.visited = True
                result.append(node.label)
    return result
```

## DFS

DFS算法应用了递归的思想，用大白话讲：从节点a开始遍历图，等于从节点a的邻居开始分别遍历图（跳过已经被经过过的节点）

```python
#coding:utf-8
class Edge:
    def __init__(self, neighbour=None):
        self.neighbour = neighbour

class Vertex:
    def __init__(self,label):
        self.neighbours = []
        self.visited = False
        self.label =label

class Graph:
    def __init__(self):
        self.vertexs = []
        self.edges = []

    def addVertex(self, label):
        vertex = Vertex(label)
        self.vertexs.append(vertex)
        return vertex

    def addEdge(self, source, dest):
        edge = Edge(dest)
        source.neighbours.append(edge)


def DFS(source):
    result = [source.label]
    source.visited = True
    for edge in source.neighbours:
        if not edge.neighbour.visited:
            result += DFS(edge.neighbour)
    return result
```

## 应用
图的遍历（尤其是dfs）是非常常见的算法。

[leetcode 39题](https://leetcode.com/problems/combination-sum/) 就是一个可以用dfs解决的问题。题目给定一个数字集合，比如[2,3,6,7]，和一个目标数据，比如7，解集为[[7], [2,2,3]]。此题我们就可以用dfs来解

```python
class Solution(object):
    def combinationSum(self, candidates, target):
        res = []
        self.dfs(candidates, target, 0, [], res)
        return res

    def dfs(self, cands, target, ind, path, res):
        if target == 0:
            res.append(path)
            return
        if target < 0:
            return
        for i in range(ind, len(cands)):
            self.dfs(cands, target - cands[i], i, path+[cands[i]], res)

```

[leetcode 46题](https://leetcode.com/problems/permutations/) 求全排列，也可以用dfs来求解。

```python
class Solution:
    def permute(self,nums):
        res = []
        self.dfs(nums, [], res)
        return res
    def dfs(self, nums, path, res):
        if not nums:
            res.append(path)
            return
        for i,n in enumerate(nums):
            self.dfs(nums[:i]+nums[i+1:],path+[n],res)
```
