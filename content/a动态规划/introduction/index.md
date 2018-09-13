---
title: 动态规划
prev: /a动态规划

weight: 5
---

我们先来实现最常见的斐波那契数列。[1,1,2,3,5,8,11 ...]，我们知道从第三个数开始，存在这样的规律fib(n) = fib(n-1) + fib(n-2)

我们可以很自然的写出这样的函数

```python
def fib(n):
    if n == 0 or n == 1:
        return 1
    if n > 1:
        return fib(n-1)+fib(n-2)
```

试试用上面的函数计算fib(40)，算了很久吧，再试试fib(41)呢，不用算了，fib(41)=fib(40)+fib(39)，计算的时间就是fib(40)的时间和fib(39)的时间。这么递归下去，我们如果要算fib(100)怎么办？

方法是把递归变成非递归算法，我们把计算fib(100)变成计算fib(99)+fib(98),而fib(99)=fib(98)+fib(97)，在计算fib(100)和fib(99)时，我们发现都需要计算fib(98),那么我们应该在计算得到fib(98)，时就把它存下来，当需要用到的时候，不要再去计算一遍。

```python
def fib(n):
    if n == 0 or n == 1:
        return 1
    ret = [1,1]+[0]*(n-2)
    for i in range(2,n):
        ret[i] = ret[i-1]+ret[i-2]
    return ret[-1]
```
现在我就算是要计算fib(1000)也是瞬间就可以得到结果了。事实上如果我们不需要整个数列，而是只要求fib(n)的话，我们还可以简化代码，并降低空间复杂度。我们并不需要计算出整个数列，只需要保存当前位置的前两个数的值。

```python
def fib(n):
    if n < 2:
        return 1
    a,b = 1,1
    for _ in range(n-2):
        a,b = b,a+b
    return b
```


## 爬梯子问题

[leetcode 70题](https://leetcode.com/problems/climbing-stairs/) 这道题是斐波那契数列的一个变种。题目说，如果你要爬一个梯子，一次可以爬一级或者两级，你有几种爬法。

显然，如果梯子只有一级那就是一种，如果有两级，那就有两种。如果有三级，你可以先爬一级，这个时候问题就变成了你有几种方法爬上两级梯子；你也可以先爬两级，这个时候问题变成了你有几种方法爬上一级梯子。p(3)=p(1)+p(2),而对于所有大于等于3的n，我们都有p(n)=p(n-2)+p(n-1)。代码也和上面的斐波那契数列几乎一样。

```python
class Solution(object):
    def climbStairs(self, n):
        if n==1:
            return 1
        if n==2:
            return 2
        a,b=1,2
        for _ in range(n-2):
            a,b = b,a+b
        return b
```

## unique paths问题
[leetcode 62题](https://leetcode.com/problems/unique-paths/) 这道题是非常经典非常典型的动态规划题。问题是从一个方阵的左上角走到右下角有多少种走法（每次只能向右或者向下）。首先，到达方阵上边缘各个点的路径数都是1，到达方阵左边缘各个点的路径数都是1，而到达其他点都可以从其左侧进入或者从其上侧进入。也就是 `$grid[i][j] = grid[i-1][j] + grid[i][j-1]$`。

```python
class Solution(object):
    def uniquePaths(self, m, n):
        grid = [[0]*n]*m
        for i in range(m):
            grid[i][0] = 1
        for i in range(n):
            grid[0][j] = 1
        for i in range(1,m):
            for j in range(1,n):
                grid[i][j] = grid[i-1][j] + grid[i][j-1]
        return grid[-1][-1]
```

## unique paths II问题
[leetcode 63题](https://leetcode.com/problems/unique-paths-ii/) 这道题是上一道题的进化版，在方阵中加上一些障碍。还是计算左上到右下的路径数，但是所有的路径都不能经过这些有障碍的点。

```python
class Solution(object):
    def uniquePathsWithObstacles(self, obstacleGrid):
        m,n = len(obstacleGrid), len(obstacleGrid[0])
        obstacleGrid[0][0] = 1 - obstacleGrid[0][0]

        for i in range(1, n):
            if not obstacleGrid[0][i]:
                obstacleGrid[0][i] = obstacleGrid[0][i-1]
            else:
                obstacleGrid[0][i] = 0

        for i in range(1, m):
            if not obstacleGrid[i][0]:
                obstacleGrid[i][0] = obstacleGrid[i-1][0]
            else:
                obstacleGrid[i][0] = 0

        for i in range(1, m):
            for j in range(1, n):
                if not obstacleGrid[i][j]:
                    obstacleGrid[i][j] = obstacleGrid[i][j-1]+obstacleGrid[i-1][j]
                else:
                    obstacleGrid[i][j] = 0

        return obstacleGrid[-1][-1]
```

## minimum path sum问题
[leetcode 64题](https://leetcode.com/problems/minimum-path-sum/) 这个题也是超级经典的动态规划问题，要求计算从左上到右下的最短路径长度。我们还是用动态规划的套路来解。首先计算上边缘和左边缘，然后对于其他点来说，看上边到达比较近还是左边到达比较近，来决定其距离。


```python
class Solution(object):
  
    def minPathSum(self, grid):
        m = len(grid)
        n = len(grid[0])
        for i in range(1, n):
            grid[0][i] += grid[0][i-1]
        for i in range(1, m):
            grid[i][0] += grid[i-1][0]
        for i in range(1, m):
            for j in range(1, n):
                grid[i][j] += min(grid[i-1][j], grid[i][j-1])
        return grid[-1][-1]
```

## decode ways问题
[leetcode 91题](https://leetcode.com/problems/decode-ways/)
