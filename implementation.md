# Larry's Array

<https://www.hackerrank.com/challenges/larrys-array/problem>

rotate操作定义为：三个循环左移1。比如ABC连续进行ratate操作可以得到：BCA, CAB, ABC。

给定一个由1~n组成的长度为n的数组，如果能通过若干次rotate操作（每次rotate必须是相邻的三个数）将其排序，则输出'YES'，否则输出'NO'。

**思路**

改变数组中两个数的先后顺序称为reverse。对于数组['A','B','C']，其中包含三个序列对'AB'，'AC'和'BC'，仔细观察由他rotate得到的两个序列BCA和CAB。例如BCA，包含的序列对有'BC'，'BA'和'CA'，相对于'ABC'来说reverse发生了两次，CAB也是一样。也就是说，每次rotate过程发生reverse的次数只能是0次或者2次。

遍历数组中的所有序列对，如果他们是逆序的，也就是说前面的数比后面的数大，那么他们一定要经过至少一次（奇数次）reverse才能变成升序。如果reverse的次数为偶数，那么就能通过rotate排序，如果是奇数则不能。**至于为什么偶数个逆序对一定能通过rotate排序，没有去详细证明。**

类似算法问题为：[15-puzzle](https://en.wikipedia.org/wiki/15_puzzle)

```python
def larrysArray(A):
    numReverse = 0
    for i in range(len(A)-1):
        for j in range(i,len(A)):
            if A[i]>A[j]:
                numReverse += 1
    if numReverse%2==0:
        return 'YES'
    else:
        return 'NO'
```

# Ema's Supercomputer

<https://www.hackerrank.com/challenges/two-pluses/problem>

**思路**

先列出矩阵中的所有“加号”，然后两两判断是否有重叠部分，取没有重叠的“加号”对的乘积的最大值。

```python
class Cross:
    def __init__(self,row,col,margin):
        self.row = row
        self.col = col
        self.margin = margin
    def collid(self,cross):
        s = set()
        for j in range(self.col-self.margin,self.col+self.margin+1):
            s.add((self.row,j))
        for i in range(self.row-self.margin,self.row+self.margin+1):
            s.add((i,self.col))
        for j in range(cross.col-cross.margin,cross.col+cross.margin+1):
            if (cross.row,j) in s:
                return True
        for i in range(cross.row-cross.margin,cross.row+cross.margin+1):
            if (i,cross.col) in s:
                return True
        return False

def isCross(grid,row,col,margin):
    for j in range(col-margin,col+margin+1):
        if grid[row][j]=='B':
            return False
    for i in range(row-margin,row+margin+1):
        if grid[i][col]=='B':
            return False
    return True

def twoPluses(grid):
    m,n = len(grid),len(grid[0])
    maxMargin = (min(m,n)-1)//2
    crosses = []
    for margin in range(maxMargin+1):
        for i in range(margin,m-margin):
            for j in range(margin,n-margin):
                if isCross(grid,i,j,margin):
                    crosses.append(Cross(i,j,margin))
    result = 0
    for i in range(len(crosses)-1):
        for j in range(i,len(crosses)):
            if not crosses[i].collid(crosses[j]):
                result = max(result,(crosses[i].margin*4+1)*(crosses[j].margin*4+1))
    return result
```

# Almost Sorted

<https://www.hackerrank.com/challenges/almost-sorted/problem>

将一个数组（所有数互不相同）排序成升序数组，每次操作只能是下面的操作之一：

- 交换两个数（swap）
- 将一个子序列翻转顺序（reverse）

如果数组本身有序，则输出“YES”；如果这个数组能通过**一次**上面的操作之一变成升序，则输出“YES”并且输出操作的内容（swap优先于reverse）；如果不能通过一次操作使其变成升序，则输出“NO”。

数组的长度最大为100000，数组中的每个元素唯一。

**思路**

先将数组排序，得到有序状态每个数应该在的位置。对比有序数组和原始数组，记录下所有不相同的数的下标：

- 如果所有数相同，则不用改动；
- 如果有两个数不相同，则通过一次swap可以将其变为有序；
- 如果多余两个数不相同，则可能情况为翻转后有序或者不能使其变为有序。这个时候只需要比较不相同部分，如果他们的这一部分为逆序关系，则通过reverse可以排序，否则不能。

```py
def almostSorted(arr):
    sortedArr = arr[:]
    sortedArr.sort()
    diffs = []
    for i in range(len(arr)):
        if arr[i]!=sortedArr[i]:
            numDiff += 1
            diffs.append(i)
    if len(diffs) == 0:
        print("yes")
    elif len(diffs) == 2:
        print("yes")
        print("swap "+str(diffs[0]+1)+" "+str(diffs[1]+1))
    else:
        for j in range((diffs[-1]-diffs[0]+1)//2+1):
            if arr[diffs[0]+j]!=sortedArr[diffs[-1]-j]:
                print("no")
                return
        print("yes")
        print("reverse "+str(diffs[0]+1)+" "+str(diffs[-1]+1))
```

# Matrix Layer Rotation

<https://www.hackerrank.com/challenges/matrix-rotation-algo/problem>

给定一个m x n的矩阵，逆时针将其旋转r次，输出旋转后的矩阵。

旋转的方式是：将矩阵看成一圈一圈的数组成的，旋转的时候所有圈同时旋转。

**思路**

```python
#!/bin/python3

import math
import os
import random
import re
import sys

# Complete the matrixRotation function below.
def matrixRotation(matrix, r):
    m = len(matrix)
    n = len(matrix[0])
    ncircle = (min(m,n)+1)//2
    circlesizes = [(m-2*i,n-2*i) for i in range(ncircle)]
    resultMatrix = []
    for i in range(m):
        row = []
        for j in range(n):
            c = min(i,j)
            size = circlesizes[c]
            innerRow,innerCol = i-2*c,j-2*c
            trans = rotate(size,innerRow,innerCol)
            row.append(matrix[i+trans[0]][j+trans[1]])
        resultMatrix.append(row)
    return resultMatrix


def rotate(size,row,col,r):
    m,n = size
    r = r % (2*(m+n)-4)
    trans = [0,0]
    k = 0
    if col == 0:
        k = row
    elif row == m-1:
        k = m - 1 + col
    elif col = n-1:
        k = 2*m+n-3-row
    else:
        k = 2*m + 2*n - 4 - col
    k = (k + r)%(2*m+2*n-4)
    if k <=m-1:
        trans = (k,0)
    elif k <= m+n-2:
        trans = (m-1,k-m)
```