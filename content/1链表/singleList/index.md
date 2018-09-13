---
title:  单链表
prev: /1链表
next: /1链表/dlist
weight: 5
---
### 简介

单链表顾名思义就是一个链式数据结构，它有一个表头，并且除了最后一个节点外，所有节点都有其后继节点。如下图。
![singleLinkedList](/img/ch1/singleLinkedList.png)

### 单链表的实现

本节我们用python来实现单链表。首先，我们写出链表节点的类。单链表中的每一个节点，都保存其数据域和后驱指针。

```python
class Node:
    def __init__(self,val,next=None):
        self.data = val
        self.next = next
```

下面我们写出单链表的基本框架，其中主要包括单链表的初始化函数，单链表的节点访问与修改，以及单链表的节点插入与删除。
```python
class LinkedList:
    def __init__(self, data=None):
        pass

    def __len__(self):
        pass

    def __getitem__(self, index):
        pass

    def __setitem__(self, index, value):
        pass

    def is_empty(self):
        pass

    def clear(self):
        pass

    def insert(self, index, data):
        pass

    def delete(self, index):
        pass
```

首先是初始化一个链表,传入一个list并将其所有元素依次串联成一个链表。
注意，链表对象并不持有所有元素，它只保存了表头。
```python
def __init__(self, data=None):

    if data == None:
        self.head = None
    else:
        self.head = Node(data[0])
        p = self.head
        for i in data[1:]:
            node = Node(i)
            p.next = node
            p = p.next
```

想要知道链表有多长？你必须遍历单链表，时间复杂度为`$O(n)$`
```python
def __len__(self):

    if self.head is None:
        return 0
    p = self.head
    result = 0
    while p != None:
        p = p.next
        result += 1
    return result
```

判断链表是否为空，以及清空链表，都非常简单
```python
def is_empty(self):
    return self.head == None

def clear(self):
    self.head = None
```

想要索引单链表怎么办,还是得从头一个一个数过去
```python
def __getitem__(self, index):

    if index < 0 or index >= len(self):
        return None
    if self.head is None:
        return None
    p = self.head
    j = 0
    while j < index:
        p = p.next
        j += 1
    return p.data

def __setitem__(self, index, value):

    if index < 0 or index >= len(self):
        return
    if self.head is None:
        return
    p = self.head
    j = 0
    while j < index:
        p = p.next
        j += 1
    p.data = value
```

链表中还有两个特别重要的方法，插入和删除。插入需要找到插入的位置，把前一个元素的next指针指向被插入的节点，并将被插入节点的next指针指向后一个节点，如下图左侧所示。而删除则是把前一个节点的next指针指向后一个节点，并返回被删除元素的数据内容，如下图右侧所示。

![insert_delete](/img/ch1/insert_delete.png)

```python
def insert(self, index, data):

    if data is None:
        return
    if index < 0 or index > len(self):
        return

    if index == 0:
        q = Node(data, self.head)
        self.head = q
        return

    p = self.head
    post = self.head
    j = 0
    while j < index:
        post = p
        p = p.next
        j += 1
        if index == j:
            q = Node(data, p)
            post.next = q


def delete(self, index):
  
    if index < 0 or index >= len(self):
        return None
    if self.head is None:
        return None
    if index == 0:
        result = self.head.data
        self.head = self.head.next
        return result
    p = self.head
    j = 0
    while j < index-1:
        p = p.next
        j += 1
    result = p.next.data
    p.next = p.next.next
    return result
```
