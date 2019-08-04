# Ahead of All

有的问题在使用递归的过程中会有很多重复的部分，比如斐波那契问题；使用动态规划的思路就是将求解过程记录下来，当遇到已解决问题的时候直接给出答案。

# The Coin Change Problem

<https://www.hackerrank.com/challenges/coin-change/problem>

给定一些硬币的面值，和一个目标数额，求用这些面值的硬币组成目标数额一共有多少种组合方式。

**思路**

选取coins中的任意一枚硬币X，其面值为x，那么使用coins来凑够target数额的方式有：

![](https://latex.codecogs.com/gif.latex?F%28coins%2Ctarget%29%3DF%28coins-%7BX%7D%2Ctarget%29&plus;F%28coins%2Ctarget-x%29)

因为无非就是两种情况，选取X或者不选取X，选取X的话coins集合不变，target要减去x；而如果不选取X的话，coins集合要减去X，target不变。

根据这一思路仅使用递归，而不使用动态规划的话代码为：

```python
def getWays(target,coins):
    if target==0:
        return 1
    elif target<0 or len(coins)<1:
        return 0
    else:
        deset,sameset = coins.copy(),coins.copy()
        randCoin = deset.pop()
        return getWays(target-randCoin,sameset)+getWays(target,deset)
```

这一段代码能通过部分测试用例，不通过的部分都是因为超时。

使用动态规划方法的思路是：创建一个target+1行，numCoin+1列的矩阵dp，其中target表示目标金额，numCoin表示使用到的金币的种类；dp[t][c]表示使用前c种硬币凑出t的方案数量；第0行除了第0个元素之外，其他都应该设置为1，因为只有一种方案（不使用任何硬币）；第0列所有数都应该设置为0，因为不使用硬币的话是凑不出来任何目标数值的。

求矩阵种其他位置的值的方法和递归类似，需要考虑两种情况：使用当前硬币，或者不使用。

当t >= c的时候，`dp[target][coin] = dp[target-recentCoin][coin] + dp[target][coin-1]`

否则，`dp[target][coin] = dp[target][coin-1]`

```python
def getWays(n, c):
    # dp[target][coin]
    dp = [[0]*(len(c)+1) for _ in range(n+1)]
    for coin in range(len(c)+1):
        dp[0][coin] = 1
    for target in range(n+1):
        dp[target][0] = 0
    for target in range(1,n+1):
        for coin in range(1,len(c)+1):
            recentCoin = c[coin-1]
            if target>=recentCoin:
                dp[target][coin] = dp[target-recentCoin][coin] + dp[target][coin-1]
            else:
                dp[target][coin] = dp[target][coin-1]
    return dp[n][len(c)]
```
