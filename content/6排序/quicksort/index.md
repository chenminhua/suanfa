---
date: 2016-04-09T16:50:16+02:00
next: /7图算法/
prev: /6排序/mergesort
title: 快速排序
toc: true
weight: 25
---

快速排序的平均运行时间是`$O(NlogN)$`。但是其最坏情况可能达到`$O(N^2)$`。我们需要选择一个元素作为pivot,然后把大于pivot的和小于pivot的分开为两个数组，并各自进行分类。

```python
def q_sort(l):
    if len(l) <= 1:
        return l
    pivot = l[0]
    left = [x for x in l[1:] if x < pivot]
    right = [x for x in l[1:] if x > pivot]
    return q_sort(left) + [pivot] + q_sort(right)

#测试
import random
a = [random.random() for i in range(40)]
print q_sort(a)
```

快速排序也可以一行写出来
```python
q_sort = lambda l : l if len(l) <= 1 else q_sort([x for x in l[1:] if x < l[0]]) + [l[0]] + q_sort([x for x in l[1:] if x > l[0]])
```
