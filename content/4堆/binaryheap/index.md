---
title: 二叉堆
prev: /4堆
next: /4堆/commonquestion
weight: 5
---

二叉堆是一棵被完全填满的二叉树。它的最重要的特性则是堆序性。

所谓堆序是指：对于所有节点儿子都比父亲大，或者对于所有节点儿子都比父亲小。这种性质可以保证出队的时间复杂度为O(1)。

## 最小堆
要形成最小堆，我们就要不断维持堆序。在新元素入堆的过程中，我们先将一个空穴添加到最后，然后不断对其进行"上滤"操作直到找到插入位置。而当我们删除最小元素（堆顶）时，我们可以把堆首元素变成空穴，然后"下滤"到合适位置。并把堆尾元素移入。



```python
class Heap:
    ##最小堆（父亲比儿子小）
    def __init__(self, datas=[]):
        pass

    def buildheap(self):
        for i in range(len(self) / 2 - 1, -1, -1):
            self.percolateDown(i)

    def percolateDown(self, i):
        tmp = self[i]
        while i*2+1 < len(self):
            child = 2*i+1
            if child+1 < len(self) and self[child+1] < self[child]:
                child += 1
            if self[child] < tmp:
                self[i] = self[child]
                i = child
            else:
                break
        self[i] = tmp

    def __len__(self):
        return len(self.datas)

    def isEmpty(self):
        return len(self) == 0

    def findMin(self):
        return self[0] if not self.isEmpty() else None

    def deleteMin(self):
        if self.isEmpty():
            return None
        minItem = self[0]
        self[0] = self.datas.pop()
        self.percolateDown(0)
        return minItem

    def insert(self, data):
        hole = len(self)
        self.datas.append(data)
        while hole > 0 and data < self[(hole+1)/2-1]:
            self[hole] = self[(hole + 1)/2 -1]
            hole = (hole+1)/2-1
        self[hole] = data

    def __getitem__(self,i):
    	return self.datas[i]

    def __setitem__(self,i,y):
    	self.datas[i] = y
```

## 测试
写完了上面的堆结构，我们怎么能保证我们的代码是正确的呢。一个好的编程习惯是在编写业务代码前，先写出高质量的测试代码。好的测试代码应该尽可能的覆盖边界条件，这里为了简化代码，我们只写了简单的测试样例。我们使用nose测试框架来写我们的测试。将下面的代码添加到上面的堆结构后面

```python
from nose.tools import assert_equal

def test():
    h = Heap([32,68,19,65,24,26,13,21,16,31])
    assert_equal(h.datas, [13, 16, 19, 21, 24, 26, 32, 68, 65, 31])
    h.insert(14)
    assert_equal(h.datas, [13, 14, 19, 21, 16, 26, 32, 68, 65, 31, 24])
    assert_equal(h.deleteMin(),13)
    assert_equal(h.datas, [14, 16, 19, 21, 24, 26, 32, 68, 65, 31])

    print "success"

test()
```

运行后如果输出"success"则表明我们的代码准确无误。记住，我们应该在编写业务代码前编写测试代码。怎么样，是不是感觉已经可以在简历上骄傲地写下熟悉TDD开发了。
