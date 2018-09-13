---
title: 队列
prev: /2栈和队列/stack/
next: /2栈和队列/commonquestion
weight: 10
---

这里我们只是简单实现一下队列的api,在不考虑性能的情况下，用python的list完全可以实现

## 简单队列

**加个图**

队列主要的api包括入队，出队，以及查看队首元素
```python
class Queue:
    def __init__(self,datas=[]):
        #用一个list保存队列数据
        self.datas = datas

    def __len__(self):
        #计算队列数据长度
        return len(self.datas)

    def isEmpty(self):
        #判断队列是否为空
        return len(self) == 0

    def enqueue(self, data):
        #入队
        self.datas.append(data)

    def dequeue(self):
        #出队
        return self.datas.pop(0) if not self.isEmpty() else None

    def peek(self):
        #查看队首元素
        return self[0] if not self.isEmpty() else None
```

## 双端队列

**加个图**

和简单队列不同的是，双端队列两边都可以插入和删除。
```python
class Deque:
    def __init__(self,datas=[]):
        self.datas = datas

    def __len__(self):
    	return len(self.datas)

    def isEmpty(self):
    	return len(self) == 0

    def enqueue(self, data):
        self.datas.append(data)

    def dequeue(self):
        return self.datas.pop(0) if not self.isEmpty() else None

    def enqueueFront(self, data):
        #头部入队
        self.datas.insert(0, data)

    def dequeueBack(self):
        #头部出队
        return self.datas.pop() if not self.isEmpty() else None

    def peekFront(self):
        return self[0] if not self.isEmpty() else None

    def peekBack(self):
    	return self[len(self)-1]  if not self.isEmpty() else None
```

## Queue和deque
[Queue官方文档](https://docs.python.org/2/library/queue.html)，这是标准库里的队列

[deque官方文档](https://docs.python.org/2.7/library/collections.html#collections.deque)，这是collections包里的双端队列

这两个类都很容易上手。
