# Ahead of All

动态规划通常是配合递归来出题的。

有的问题在使用递归的过程中会有很多重复的部分，比如斐波那契问题；使用动态规划的思路就是将求解过程记录下来，当遇到已解决问题的时候直接给出答案。

# The Coin Change Problem

<https://www.hackerrank.com/challenges/coin-change/problem>

给定一些硬币的面值，和一个目标数额，求用这些面值的硬币组成目标数额一共有多少种组合方式。

**思路**

不考虑边界条件的话，求解函数应该定义为：
