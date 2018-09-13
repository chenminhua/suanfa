---
title: 常见问题
prev: /2栈和队列/queue/
next: /3树
weight: 15
toc: true
---

## valid Parenthese
[leetcode 20题](https://leetcode.com/problems/valid-parentheses/) 题目要求我们判断一个字符串是否为有效的括号组。"{[]()()}"是一个有效的括号组，而"{()[}"并不是。

我们可以用栈来帮助我们实现。当我们遇到左括号的时候我们将其压入栈中，当我们遇到右括号的时候，我们让栈顶元素出栈，并查看得到的左括号是否和当前的右括号匹配，如果遇到右括号而当前栈为空的话，显然也可以直接判断这个字符串不是有效的括号组


```python
class Solution(object):
    def isValid(self, s):
        dict = {"}":"{","]":"[",")":"("}
        stack = []
        for st in s:
            if st in dict.values():
                stack.append(st)
            elif len(stack)>0:
                p = stack.pop()
                if p != dict[st]:
                    return False
            else:
                return False
        return stack == []
```

## basic calculator
[leetcode 224题](https://leetcode.com/problems/basic-calculator/)
题目要求实现一个能够计算公式值的函数。比如输入"1 + 1"返回2，输入"(1-(4-5+2)-3)+(6+8)"返回11，只需要支持加法减法和括号就可以了。

比较特殊的就是括号，我们知道，括号里面的运算具有优先级，但是在解析字符串的时候，我们从左往右依次解析，所以我们不可能提前知道哪部分需要被先计算。我们可以在遇到左括号的时候，对当前数值进行入栈，并将括号前的符号也入栈。比如在"(1-(4-5+2)-3)+(6+8)"我们在解析到第二个左括号的时候，就把当前计算到的1入栈，然后把负号也入栈。而当我们解析到第一个右括号的时候，我们已经计算出括号内的值为1，我们将此时处于栈顶的负号出栈并乘上括号内的结果1。然后栈顶元素1出栈加上-1得到0。我们重复这个过程知道字符串解析完成。

```python
class Solution(object):
    def calculate(self, s):
        res = 0
        num = 0
        sign = 1
        stack = []
        for c in s:
            if c.isdigit():
                num = 10 * num + int(c)
            elif c in "+-":
                res += sign * num
                num = 0
                sign = 1 if c=="+" else -1
            elif c == "(":
                stack.append(res)
                stack.append(sign)
                sign,res = 1,0
            elif c == ")":
                res += sign * num
                res *= stack.pop()
                res += stack.pop()
                num = 0
        return res + num*sign
```
