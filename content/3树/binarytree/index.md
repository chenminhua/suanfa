---
title: 二叉树
prev: /3树/
next: /3树/avltree
weight: 5
---

二叉树是一种非常常见常用（常考）的数据结构。二叉树的特点是每个节点最多有两个子女，分别称为左子女和右子女。下图就是一棵二叉树。节点 1 我们称作根节点。而 4，5，6 我们称作叶子节点。
![binarytree](/img/ch3/binarytree.png)

## 二叉搜索树

二叉搜索树是二叉树的子集，其特点是对于树中的每一个节点，其左子树中的所有元素都比它小，右子树中的所有元素都比它大。如下图，左侧的就是一棵二叉搜索树;而右侧的则不是，因为节点 9 在根节点 10 的右子树上，但是其值却比 10 小.
![bst](/img/ch3/bst1.png)

## 二叉搜索树的实现

首先我们写出树节点的类，它包含三个属性：数据，左子树，右子树

```python
class Node:
    def __init__(self, element, left=None, right=None):
        self.element = element
        self.left = left
        self.right = right
```

下面我们写出二叉搜索树的基本框架

```python
class BinarySearchTree:
    def __init__(self, root=None):
        self.root = root  #初始化函数

    def clear(self):
        self.root = None  #清空树

    def isEmpty(self):
        return self.root == None  #判断树是否为空

    def contain(self, x, node):
        pass  #查找某元素

    def findMin(self, node):
        pass  #找最小元素

    def findMax(self, node):
        pass  #找最大元素

    def insert(self, x, node):
        pass  #插入一个元素

    def remove(self, x, node):
        pass  #删除一个元素

    def print_tree(self, node):
        pass  #中序遍历

    def post_order_print_tree(self, node):
        pass  #后序遍历

    def pre_order_print_tree(self, node):
        pass  #前序遍历
```

## 树的遍历

树三种遍历方法:中序遍历，前序遍历和后序遍历。对于下面的树来说，

前序遍历的结果为: 10 8 3 16 13 18

中序遍历的结果为: 3 8 10 13 16 18

后序遍历的结果为: 3 8 13 18 16 10

![bst2](/img/ch3/bst2.png)

```python
def print_tree(self, node):
    if node != None:
        self.print_tree(node.left)
        print node.element
        self.print_tree(node.right)

def post_order_print_tree(self, node):
    if node != None:
        self.post_order_print_tree(node.left)
        self.post_order_print_tree(node.right)
        print node.element

def pre_order_print_tree(self, node):
    if node != None:
        print node.element
        self.pre_order_print_tree(node.left)
        self.pre_order_print_tree(node.right)
```

下面我们来写 contain 方法，查找一棵树里面是否存在某个节点。从根节点开始，如果待查找的值比当前节点大，我们就去当前节点右子树找，如果比当前节点小，我们就去当前节点的左子树去找，直到找到目标(返回 True)或者找到叶子节点处仍没有找到(返回 False)。

```python
def contain(self, x, node):
    if node is None:
        return False
    if x < node.element:
        return self.contain(x, node.left)
    elif x > node.element:
        return self.contain(x, node.right)
    else:
        return True
```

如何找到二叉搜索树中的最大值和最小值？最大值就是一直向右子树找过去直到叶子节点。而最小值则是一直向左子树找过去直到找到叶子节点。

```python
def findMin(self, node):
    if node == None:
        return None
    elif node.left == None:
        return node
    return self.findMin(node.left)


def findMax(self, node):
    if node == None:
        return None
    elif node.right == None:
        return node
    return self.findMax(node.right)
```

如何向二叉搜索树插入一个节点？以下图为例，我们要插入一个节点 17,从根节点开始，17 大于 10，所以去右子树插入。17 大于 16，继续往右。来到 18，17 小于 18 所以往左。而 18 恰好没有左儿子，所以把 17 插在 18 的左侧。

![bst2](/img/ch3/insert.png)

```python
def insert(self, x, node):
    #不考虑相同元素出现
    if node == None:
        return Node(x)
    if x < node.element:
        node.left = self.insert(x,node.left)
    elif x > node.element:
        node.right = self.insert(x,node.right)
    return node
```

删除则稍微来的麻烦一点，假设我们要删除下图左侧的 16，我们首先来到 16 这个节点，然后我们从右子树中找到一个最小的元素 17（其必然是一个叶子节点），把这个叶子节点 17 替换到 16 的位置并将叶子节点删除。

![remove](/img/ch3/remove.png)

```python
def remove(self, x, node):
    if node == None:
        return node
    if x < node.element:
        node.left = self.remove(x, node.left)
    elif x > node.element:
        node.right = self.remove(x, node.right)
    elif node.left != None and node.right != None:
        node.element = self.findMin(node.right).element
        node.right = self.remove(node.element, node.right)
    else:
        node = node.left if (node.left is not None) else node.right
    return node
```

## 性能分析

二叉搜索树的查找，插入和删除的时间复杂度都和树高成正比，所以时间复杂度都是`$O(logn)$`,其中 n 为节点数。
