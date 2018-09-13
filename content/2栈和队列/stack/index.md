---
title: 栈
prev: /2栈和队列
next: /2栈和队列/queue
weight: 5
---

这里我们只是简单实现一下栈的api,在不考虑性能的情况下，用python的list完全可以实现。

**加个图**

```python
class Stack:
    def __init__(self,datas=[]):
        #用一个list来保存栈的数据
        self.datas = datas

    def __len__(self):
        #计算栈内数据个数
        return len(self.datas)

    def isEmpty(self):
        #判断栈是否为空栈
        return len(self) == 0

    def push(self,data):
        #压栈，将一个元素压入栈中
        self.datas.append(data)

    def pop(self):
        #栈顶元素出栈，如果栈为空栈则返回None
        return self.datas.pop() if not self.isEmpty() else None

    def peek(self):
        #查看栈顶元素
        return self[len(self.datas)-1] if not self.isEmpty() else None
```
