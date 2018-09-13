---
title: 希尔排序
prev: /6排序/insertsort
next: /6排序/heapsort
weight: 10
---

希尔排序是冲破二次时间屏障的第一批算法。它通过比较相距一定间隔的元素来工作，且各趟比较所用的距离随着算法的进行而减小。所以希尔排序有时候也叫缩减增量排序。

我们还是举一个例子来分析,假设有一个数组 81,99,12,33,43,23,48,76,68,11,25,87,90。每一步，我们都设定一个增量，并根据增量将数组分组，并对每一个分组进行插入排序。

运行步骤 | 结果
--- | ---
原始序列 |  81  99  12  33  43  23  48  76  68  11  25  87  90
按5为距离 | 23  48  12  33  11  25  87  76  68  43  81  99  90
按3为距离 | 23  11  12  33  48  25  43  76  68  87  81  99  90
按1为距离 | 11  12  23  25  33  43  48  68  76  81  87  90  99


```python
def insertSort(l, start, gap):
    for i in range(start+gap, len(l), gap):
        currentValue = l[i]
        pos = i
        while pos >= gap and l[pos-gap] > currentValue:
            l[pos] = l[pos - gap]
            pos -= gap
        l[pos] = currentValue

def shellSort(l):
    sublistCount = len(l) / 2
    while sublistCount>0:
        for pos in range(sublistCount):
            insertSort(l, pos, sublistCount)
        sublistCount = sublistCount / 2

#测试
import random
a = [random.random() for i in range(40)]
print a
shellSort(a)
print a


```

## 性能分析
虽然希尔排序编程简单，但是，其运行时间的分析相当复杂。而且不同的增量选择方式对运行时间也有着明显的影响  
