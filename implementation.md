# Larry's Array

<https://www.hackerrank.com/challenges/larrys-array/problem>

rotate操作定义为：三个循环左移1。比如ABC连续进行ratate操作可以得到：BCA, CAB, ABC。

给定一个由1~n组成的长度为n的数组，如果能通过若干次rotate操作（每次rotate必须是相邻的三个数）将其排序，则输出'YES'，否则输出'NO'。

# Ema's Supercomputer

<https://www.hackerrank.com/challenges/two-pluses/problem>

**思路**

先列出矩阵中的所有“加号”，然后两两判断是否有重叠部分，取没有重叠的“加号”对的乘机的最大值。

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

