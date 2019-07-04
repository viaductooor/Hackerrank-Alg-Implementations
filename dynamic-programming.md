# Ahead of All

动态规划通常是配合递归来出题的。

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