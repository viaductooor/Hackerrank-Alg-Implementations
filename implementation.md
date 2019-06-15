# Larry's Array

<https://www.hackerrank.com/challenges/larrys-array/problem>

rotate操作定义为：三个循环左移1。比如ABC连续进行ratate操作可以得到：BCA, CAB, ABC。

给定一个由1~n组成的长度为n的数组，如果能通过若干次rotate操作（每次rotate必须是相邻的三个数）将其排序，则输出'YES'，否则输出'NO'。

# Ema's Supercomputer

<https://www.hackerrank.com/challenges/two-pluses/problem>

**思路**

计算并记录长度为1~15的十字的个数，最多为2（因为需要得到的是最大的两个十字，所以相同长度的十字最多考虑两个）。

```python
#!/bin/python3

import math
import os
import random
import re
import sys

# Complete the twoPluses function below.
def twoPluses(grid):
    m,n = len(grid),len(grid[0])
    counter = {i:0 for i in range(1,min(m,n))}
    for l in range(1,n):
        margin = (l-1)//2
        for i in range(margin,m-margin):
            for j in range(0,n-l+1):
                # check horizontal boxes
                if hgood(grid,i,j,l):
                    # check vertical boxes
                    if vgood(grid,i-margin,j+margin,l):



def grid hgood(grid,row,col,length):
    for _ in range(col,col+length):
        if grid[row][_]!='G':
            return False
    return True

def grid vgood(grid,row,col,length):
    for _ in range(row,row+length):
        if grid[_][col]!='G':
            return False
    return True



if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    nm = input().split()

    n = int(nm[0])

    m = int(nm[1])

    grid = []

    for _ in range(n):
        grid_item = input()
        grid.append(grid_item)

    result = twoPluses(grid)

    fptr.write(str(result) + '\n')

    fptr.close()

```

