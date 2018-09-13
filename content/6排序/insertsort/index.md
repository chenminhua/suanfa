---
title: 插入排序
prev: /6排序
next: /6排序/shellsort
weight: 5
---

假设有这样一个序列32,14,43,22,56,41,插入排序需要执行N-1次。第p次排序后要保证第一个到第p+1个元素都是有序的。

次数 | 数组
--- | ---
原始数组 | 32,14,43,22,56,41
第一次排序 | 14,32,43,22,56,41
第二次排序 | 14,32,43,22,56,41
第三次排序 | 14,22,32,43,56,41
第四次排序 | 14,22,32,41,43,56

```python
def insertionSort(l):
    for i in range(1,len(l)):
        tmp = l[i]
        j = i
        while j>0 and tmp < l[j-1]:
            l[j] = l[j-1]
            j -= 1
        l[j] = tmp

#测试
import random
a = [random.random() for i in range(40)]
insertionSort(a)
print a
```

## 性能分析
插入排序的性能比较糟糕,时间复杂度为`$O(N^2)$`
