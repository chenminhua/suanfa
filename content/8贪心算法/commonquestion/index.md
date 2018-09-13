---
date: 2016-04-09T16:50:16+02:00
prev: /8贪心算法/prim_kruskal
next: /9分治算法
title: 常见问题
toc: true
weight: 10
icon: "<b>2. </b>"
---

## jump game
[leetcode 55题](https://leetcode.com/problems/jump-game/)
给定一组非负整数，每个数表示可以从这个位置向前跳不多于这个数的步子。判断是否可以从头跳到尾。

比如 （2,3,1,1,4） 可以从第一个位置跳到第二个位置，再跳到最后一个位置

再比如 （3,2,1,0,4） 则不能跳到最后一个位置

```python
class Solution(object):
    def canJump(self,nums):
        m = 0
        for i,n in enumerate(nums):
            if i > m:
                return False
            m = max(m, n+i)
        return True
```

## jump game II
[leetcode 45题](https://leetcode.com/problems/jump-game-ii/)
这题是上一题的升级版，假设你一定可以从头跳到尾，求最少要跳几次。我们保存两个值，一个是当前步数下能跳的最远距离，另一个是当前步数加一步条件下能到的最远距离。

```python
class Solution(object):
    def jump(self, nums):
        n, cur_max, next_max, steps = len(nums), 0, 0, 0
        for i in xrange(n):
            if i > cur_max:
                steps += 1
                cur_max = next_max
                if cur_max >= n: break
            next_max = max(next_max, nums[i] + i)
        return steps
```

## best time to buy and sell stock II
[leetcode 122题](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
这道题的条件是可以买卖多次，但是如果处于买进状态下是不能买的。
策略很简单，涨价前买进，降价前卖出。

```python
class Solution(object):
    def maxProfit(self, prices):
        flag = False
        profit = 0
        for i in range(len(prices)-1):
            if prices[i]<prices[i+1] and not flag: #升值
                profit -= prices[i]
                flag = not flag
            if prices[i]>prices[i+1] and flag:
                profit += prices[i]
                flag = not flag
        if flag:
            profit += prices[-1]
        return profit

```
