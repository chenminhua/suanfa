---
title: avl树
prev: /3树/binarytree
next: /3树/commonquestion
weight: 10
---

前面我们介绍了二叉搜索树，并分析了其性能。但是，如果我们考虑一个极端情况。现在我们有一棵二叉搜索树，只包含根节点10，然后我们依次像其插入11,12,...19。然后你会得到下面这个东西。

![longtree](/img/ch3/longtree.png)

显然，这个时候插入和删除的时间复杂度不再是`$O(logn)$`了，而是`$O(n)$`，为什么，因为这是一个披着二叉树皮的单链表啊。

## AVL树

AVL树是带有平衡条件的二叉搜索树。其平衡条件为 **每个节点的左子树和右子树的高度差最多为1**。下图中，左侧是一棵avl树，而右侧则不是。

![avl1](/img/ch3/avl1.png)

对于一棵已经满足平衡条件的树来说，插入和删除操作可能会破坏其原本的平衡。为了让树满足平衡条件，我们需要在插入的时候做一些额外的工作来保证这棵树的平衡。我们可以通过 **旋转** 来对树进行简单的修正。

首先我们强调这样一个事实：在插入完成后，只有那些从插入点到根节点的路径上的节点的平衡可能被改变。我们可以沿着这条路径上行到根节点。

我们可以把这种不平衡分为四种情况：

  1. 在左子树的左儿子上插入导致不平衡
  2. 在左子树的右儿子上插入导致不平衡
  3. 在右子树的右儿子上插入导致不平衡
  4. 在右儿子的左子树上插入导致不平衡

第一种情况和第三种情况类似，都需要做一次旋转来让树重新平衡。
![rotate1](/img/ch3/rotate1.png)

第二种情况和第四种情况则可以归为一类，需要两次旋转
![rotate2](/img/ch3/rotate2.png)

## 实现AVL树

首先我们要定义树的节点类

```python
class TreeNode:
    """根节点的深度为1，叶子节点的高度为1，非叶子节点的高度为其子树高度的最大值加1"""
    def __init__(self, data, left=None, right=None, height=1):
        self.data = data
        self.left = left
        self.right = right
        self.height = height

    def __str__(self):
        return "tree node %s with height %d" % (self.data, self.height)

def height(node):
    return node.height if node else 0
```

avl树的关键在于四种旋转调整的方法。

```python
class AVLTree:
    def __init__(self, root=None):
        self.root = root

    def insertroot(self, data):
        self.root = self.insert(data, self.root)

    def insert(self, data=None, node=None):
        if not node:
            return TreeNode(data)
        if data < node.data:
            #插在左子树上
            node.left = self.insert(data, node.left)
            if (height(node.left) - height(node.right) == 2):
                if data < node.left.data:
                    node = self.rotateWithLeftChild(node)
                else:
                    node = self.doubleWithLeftChild(node)
        else:
            #插在右子树上
            node.right = self.insert(data, node.right)
            if (height(node.right) - height(node.left) == 2):
                if data > node.right.data:
                    node = self.rotateWithRightChild(node)
                else:
                    node = self.doubleWithRightChild(node)
                    node.height = max(height(node.left), height(node.right)) + 1;
        return node

    def rotateWithLeftChild(self, k2):
        k1 = k2.left
        k2.left = k1.right
        k1.right = k2
        k2.height = max(height(k2.left), height(k2.right)) + 1
        k1.height = max(height(k1.left), height(k2)) + 1
        return k1

    def doubleWithLeftChild(self, k3):
        k3.left = self.rotateWithRightChild( k3.left );
        return self.rotateWithLeftChild(k3);

    def rotateWithRightChild(self, k1):
        k2 = k1.right;
        k1.right = k2.left;
        k2.left = k1;
        k1.height = max( height(k1.left), height(k1.right) ) + 1;
        k2.height = max( height(k1), height(k2.right)) + 1;
        return k2;

    def doubleWithRightChild(self, k1):
        k1.right = self.rotateWithLeftChild(k1.right);
        return self.rotateWithRightChild(k1);
```



![Magic](images/magic.gif?classes=shadow)
