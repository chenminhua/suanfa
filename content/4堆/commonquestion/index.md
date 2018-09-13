---
title: 常见问题
prev: /4堆/binaryheap
next: /5散列
weight: 15
toc: true
---

## Merge k Sorted Lists
[leetcode 23题](https://leetcode.com/problems/merge-k-sorted-lists/)

```python
class Solution(object):
    def mergeKLists(self, lists):

        from heapq import heappush, heappop, heapreplace, heapify
        dummy = node = ListNode(0)
        h = [(n.val, n) for n in lists if n]
        heapify(h)
        
        while h:
            v, n = h[0]
            if n.next is None:
                heappop(h)
            else:
                heapreplace(h, (n.next.val, n.next))
            node.next = n
            node = node.next

        return dummy.next

```
