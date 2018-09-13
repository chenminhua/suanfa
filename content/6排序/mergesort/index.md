---
date: 2016-04-09T16:50:16+02:00
next: /6排序/quicksort
prev: /6排序/heapsort
title: 归并排序
toc: true
weight: 20
---

归并排序以O(NlogN)的最坏情形时间运行，而且所用的比较次数几乎是最优的。它是递归算法和分治算法的一个好例子。这个算法首先把序列分成两部分并分别排序再合并起来。

```python
def mergesort(l):
    if len(l) <=1:
        return l
    mid = len(l)/2
        left = mergesort(l[:mid])
        right = mergesort(l[mid:])
    return merge(left, right)

def merge(left, right):
    res = []
    while len(left) !=0 and len(right) != 0:
        if left[0] < right[0]:
            res.append(left.pop(0))
        else:
            res.append(right.pop(0))
    res += left
    res += right
    return res

#测试
import random
a = [random.random() for i in range(40)]
print mergesort(a)
```
