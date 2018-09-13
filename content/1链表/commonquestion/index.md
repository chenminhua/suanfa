---
title: 常见问题
prev: /1链表/dlist
next: /2栈和队列/
weight: 15
toc: true
---

本节我们来学习面试中几个常见的链表问题，题目来源主要为leetcode

## 检测链表中是否有环
[题目来源](https://leetcode.com/problems/linked-list-cycle-ii/)

基本思路：我们可以用两个指针从表头开始，一快一慢的遍历链表，快的一次走两步，慢的一次走一步。如果单链表有环，则不存在表尾(所有节点都有后继节点)，当指针进入环后，将在环里面一直转，两个指针由于一快一慢，快的指针必然会在某一个时刻"追上"慢的指针，两个指针达到同一个点。如下图中，快慢两个指针将在5-10这几个节点形成的环中进行追击，直到相遇。

所以，如果两个指针在出发后可以到达同一节点，我们就可以判断这个链表有环。

![circle_detection](/img/ch1/circle.png)

```python
class Solution(object):

    def detectCycle(self, head):
        try:
            fast = head.next
            slow = head
            while fast is not slow:
                fast = fast.next.next
                slow = slow.next
        except:
            return None
        # since fast starts at head.next, we need to move slow one step forward
        slow = slow.next

        while head is not slow:
            head = head.next
            slow = slow.next

        return head
```

## 删除当前节点
[题目来源](https://leetcode.com/problems/delete-node-in-a-linked-list/)
从前面的小节中我们已经得知，想要删除一个节点，需要把这个节点前驱节点的next指针知道其后面的节点。但是如下图，我们要删除"hello"节点，却不知道它的前驱节点的。笨办法是从链表头开始遍历找到待删除的节点。好的办法是，我们把当前节点的后继节点的数据域复制到当前节点，然后删掉后继节点。还是以下图为例，我们把第二个节点的数据域复制到第一个节点，然后删除第二个节点。

![delete_curr](/img/ch1/delete_curr.png)

```python
class Solution(object):

    def deleteNode(self, node):
        node.val = node.next.val
        node.next = node.next.next
```

## 删除从尾部数起第n个节点
[题目来源](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
我们没有办法从尾部开始遍历单链表，而且我们也不知道单链表的长度（除非我们遍历一次）。一个比较取巧的方法是用两个指针指向表头，一个先走n步，然后两个指针一起出发，当前面那个指针到达表尾的时候，后面那个指针正好处在待删除节点的前驱节点。下面就很简单啦。

以下图为例，我们想删除链表的倒数第二个节点。首先在表头设定两个指针p1,p2，并让p2先走两步，然后两个指针一同出发直到p2到达表尾。然后删除p1的后继节点（就是倒数第二个节点）。

![delete_from_end](/img/ch1/delete_from_end.png)

```python
class Solution(object):

    def removeNthFromEnd(self, head, n):

        fast = slow = head

        for _ in range(n):
            fast = fast.next

        if not fast:
            return head.next

        while fast.next:
            fast = fast.next
            slow = slow.next

        slow.next = slow.next.next
        return head
```

## 合并两个有序列表
[题目来源](https://leetcode.com/problems/merge-two-sorted-lists/) 此题比较简单，就是不停遍历两个表，并操作指针移动。
```python
class Solution(object):

    def mergeTwoLists(self, l1, l2):
        res = ListNode(0)
        p = res

        while l1 and l2:
            if l1.val < l2.val:
                p.next = l1
                p = p.next
                l1 = l1.next
            else:
                p.next = l2
                p = p.next
                l2 = l2.next

        if not l1:
            p.next = l2
        if not l2:
            p.next = l1

        return res.next
```

## 反转链表
[题目来源](https://leetcode.com/problems/reverse-linked-list/)

一图胜千言。如下图，我们有一个四个节点的单链表，我们通过如下八步，并使用了三个指针来将其反转。其中2-4步和5-7步都可以被放入一个循环中。
![reverse](/img/ch1/reverse.png)
```python
class Solution(object):

    def reverseList(self, head):
        if not head:
            return None
        if  not head.next:
            return head

        pre = head
        cur = pre.next

        while cur:
            tmp = cur.next
            cur.next = pre
            pre = cur
            cur = tmp

        head.next = None
        return pre
```
