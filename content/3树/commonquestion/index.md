---
title: 常见问题
prev: /3树/avltree
next: /4堆
weight: 15
toc: true
---

## 判断两棵树是否为同一棵
[leetcode 100题](https://leetcode.com/problems/same-tree/)

树这种数据结构，总是伴随着递归算法。在这题里面，要判断两棵树是否相同，首先要看这两棵树的根是否相同，然后看根的左子树是否相同，右子树是否相同。

```python
class Solution:
    def isSameTree(self, p,q):
        if not p or not q:
            return p == q
        return p.val == q.val and self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```

## 判断一棵树是否为对称树
[leetcode 101题](https://leetcode.com/problems/symmetric-tree/)

对称树要求左右对称，这题需要递归地检查左子树和右子树是否互相对称。
```python
class Solution(object):
    def isSymmetric(self, root):
        return self.isSym(root,root)

    def isSym(self,p,q):
        if not p and not q: return True
        if p and q:
            return p.val == q.val and self.isSym(p.left, q.right) and self.isSym(p.right,q.left)
        return False
```

## 求一棵树的最大深度
[leetcode 104题](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

求一棵树的最大深度，就是算离根最远的节点的深度，根节点的深度为1

```python
class Solution(object):
    def maxDepth(self, root):
        if not root:
            return 0
        return max(self.maxDepth(root.left), self.maxDepth(root.right))+1
```

## 求一棵树的最小深度
[leetcode 111题](https://leetcode.com/problems/minimum-depth-of-binary-tree/)

求一棵树的最小深度，就是算离根最近的叶子节点的深度。

```python
class Solution(object):
    def minDepth(self, root):
        if not root:
            return 0
        if root.left==None or root.right==None:
            return self.minDepth(root.left)+self.minDepth(root.right)+1
        return min(self.minDepth(root.right),self.minDepth(root.left))+1
```

## 判断一棵树是否为平衡二叉树
[leetcode 110题](https://leetcode.com/problems/balanced-binary-tree/)

如果所有记得的左右子树的高度差都小于等于1，那么这棵树就是平衡二叉树。我们首先要写出一个计算节点高度的函数，这个函数也是递归的。
然后根据平衡二叉树的定义，我们可以写成isBalanced()函数

```python
class Solution(object):
    def isBalanced(self, root):
        if not root:
            return True
        return abs(self.getHeight(root.left) - self.getHeight(root.right)) < 2 and self.isBalanced(root.left) and self.isBalanced(root.right)

    def getHeight(self, root):
        if not root:
            return 0
        return max(self.getHeight(root.left), self.getHeight(root.right)) + 1
```

## 二叉树的层序遍历
[leetcode 102题](https://leetcode.com/problems/binary-tree-level-order-traversal/)

这里我们需要用到辅助队列，首先队列中只有一个根，然后我们让元素出队，并将该元素的儿子入队，如此循环知道遍历完树。

```python
class Solution(object):
    def levelOrder(self, root):
        ans, level = [], [root]
        while root and level:
            ans.append([node.val for node in level])            
            level = [kid for n in level for kid in (n.left, n.right) if kid]
        return ans
```

## 从先序和中序重建二叉树
[leetcode 105题](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

先序的第一个元素告诉了我们树的根节点，我们就可以根据这个根节点在中序遍历中的位置，将其划分为左子树和右子树。

```python
class Solution(object):
    def buildTree(self, preorder, inorder):
        if inorder:
            ind = inorder.index(preorder.pop(0))
            root = TreeNode(inorder[ind])
            root.left = self.buildTree(preorder, inorder[0:ind])
            root.right = self.buildTree(preorder, inorder[ind+1:])
            return root
```

## 从后序和中序重建二叉树
[leetcode 106题](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

后序的最后一个元素告诉了我们树的根节点，我们就可以根据这个根节点在中序遍历中的位置，将其划分为左子树和右子树。

```python
class Solution(object):
    def buildTree(self, inorder, postorder):
        if inorder:
            root = TreeNode(postorder.pop())
            ind = inorder.index(root.val)
            root.right = self.buildTree(inorder[ind+1:],postorder)
            root.left = self.buildTree(inorder[:ind],postorder)
            return root      
```

## path sum
[leetcode 112题](https://leetcode.com/problems/path-sum/)

判断树中是否存在一条路径使得路径上所有节点的和为指定的值，使用递归算法

```python
class Solution(object):
    def hasPathSum(self, root, sum):
        if not root:
            return False
        if not root.left and not root.right and root.val == sum:
            return True
        return self.hasPathSum(root.left,sum-root.val) or self.hasPathSum(root.right, sum-root.val)
```

## path sum II
[leetcode 113题](https://leetcode.com/problems/path-sum-ii/)

接上一题，不过要返回所有路径和为指定值得路径。这题也是用递归的方法。事实上，下面的解法可以被总结为一个套路(在后面的章节中我们会介绍图的DFS遍历）。

```python
class Solution(object):
    def pathSum(self, root, sum):
        res = []
        self.helper(root,sum,[],res)
        return res

    def helper(self, root, sum, path, res):
        if not root:
            return
        if root.val == sum and self.isleaf(root):
            res.append(path+[sum])
            return

        self.helper(root.left, sum-root.val, path+[root.val],res)
        self.helper(root.right, sum-root.val, path+[root.val],res)


    def isleaf(self,root):
        if root.left or root.right:
            return False
        return True
```
