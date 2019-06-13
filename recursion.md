# The Power Sum

<https://www.hackerrank.com/challenges/the-power-sum/problem>

给定一个数X，将他表示成一个或多个不同的数的K次幂的和，求有多少种表示方法。

比如X=100，K=2，那么：

![](https://latex.codecogs.com/gif.latex?100&space;=&space;10^2&space;=&space;6^2&plus;8^2=1^2&plus;3^2&plus;4^2&plus;5^2&plus;7^2)

应该输出3。

**思路**

X的取值范围为[1,1000]，K的取值为[2,10]。如果可以将X表示为：

![](https://latex.codecogs.com/gif.latex?a_{1}^K&plus;a_{2}^K&plus;...&plus;a_{m}^K)

那么应该有：

![](https://latex.codecogs.com/gif.latex?1\leq&space;a_{i}\leq&space;\left&space;\lfloor&space;\sqrt[K]{X}&space;\right&space;\rfloor&space;,i=1,2,...,m)

![](https://latex.codecogs.com/gif.latex?a_{i})的取值非常有限，因此可以将所有可能的值枚举出来。现在的问题就转化为了给定多个整数，从中选取出两个或以上的整数，使其和等于X，有多少种选取方法。

**递归思想**

将函数F(target, S)定义为：从集合S中选取一个或以上的整数，使其和等于target的选取方法的总数。那么显然可以得到：

![](https://latex.codecogs.com/gif.latex?F%28target%2CS%29%3D%5Cbegin%7Bcases%7D%201%20%26%20%5Ctext%7B%20if%20%7D%20target%3D0%20%5C%5C%200%20%26%20%5Ctext%7B%20if%20%7D%20S%3D%5CO%20%5Cwedge%20target%5Cneq%200%20%5C%5C%200%20%26%20%5Ctext%7B%20if%20%7D%20target%3C0%20%5C%5C%20F%28target-a_%7Bi%7D%2CS-%5Cleft%20%5C%7B%20a_%7Bi%7D%20%5Cright%20%5C%7D%29&plus;F%28target%2CS-%5Cleft%20%5C%7B%20a_%7Bi%7D%20%5Cright%20%5C%7D%29%26%20%5Ctext%7B%20if%20%7D%20target%3E0%5Cwedge%20S%5Cneq%20%5CO%20%5Cend%7Bcases%7D)

其中![](https://latex.codecogs.com/gif.latex?a_{i})为S中的任意一个数，因为对于![](https://latex.codecogs.com/gif.latex?a_{i})只能有两种处理方法，选取或者不选取，然后取这两种方法得到的结果之和。

```python
def powerSum(X, N):
    maxa = math.floor(math.pow(X,1/N))
    if maxa == 1:
        return 0
    else:
        s = set([i**N for i in range(1,maxa+1)])
        return subConquer(X,s)

def subConquer(target,s):
    if target==0:
        return 1
    elif target<0 or len(s)==0:
        return 0
    else:
        s2 = s.copy()
        v = s2.pop()
        return subConquer(target,s2)+subConquer(target-v,s2)
```

# Crossword Puzzle

<https://www.hackerrank.com/challenges/crossword-puzzle/problem>

填字游戏。给定一个10x10的矩阵，标记了中间可以填的空，以及一些单词。将单词填入到这些矩阵中。

**思路**

先遍历一遍grid，记录下每个空的起始点位置、可填入单词长度和走向（右或者下）。

