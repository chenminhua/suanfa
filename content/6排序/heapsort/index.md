---
title: 堆排序
prev: /6排序/shellsort
next: /6排序/mergesort
weight: 15
toc: true
---

在第四章中我们就学过了堆（优先队列）这种结构。假设我们有一个数组，我们可以将数组元素依次入堆。这样在出堆的时候元素就是有序的了。关于堆得实现可以查看第四章代码。我们也可以使用Python标准库中的heapq包。

## heapq包
[官方文档](https://docs.python.org/2/library/heapq.html)

```python
#入堆
heapq.heappush(heap,item)

#出堆
heapq.heappop(heap)

#入堆一个元素并出堆一个元素
heapq.heappushpop(heap,item)

#把List变成一个堆
heapq.heapify(x)

#返回最大的n个元素
heapq.nlargest(n, iterables)

#返回最小的n个元素
heapq.nsmallest(n, iterables)
```
