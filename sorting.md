# Big Sorting

<https://www.hackerrank.com/challenges/big-sorting/problem>

给定一些表示数字的字符串（长度最多可以是![](https://latex.codecogs.com/gif.latex?10^6))，将他们排序输出。

**思路**

使用Python3实现排序的时候可以设置多个排序标准。一般参数key是一个返回单个值的函数，比如`lambda person:person.id`，但是如果有多个排序标准的话可以按照主次顺序把他们放在元组里面返回，比如`lambda person:(person.id,person.name,person.age)`就是依次根据person的id、name和age排序。

数字字符串比较大小的方式是：除去前面的'0'以外，如果两个字符串长度相等，则直接按照字符串比较的方式比较；如果长度不等，则短的字符串值更小。

```python
def bigSorting(unsorted):
    unsorted.sort(key=lambda s:(len(s),s))
    return unsorted
```

# Lily's Homework

<https://www.hackerrank.com/challenges/lilys-homework/problem>

对于一个数组arr，如果它的某一个排序的相邻元素差的绝对值的和

![](https://latex.codecogs.com/gif.latex?\sum_{i=1}^{n-1}\left&space;|&space;arr[i]-arr[i-1]&space;\right&space;|)

是所有可能的排序中最小的，那么这个排序就是beautiful的。

给定一个数组，求将他变为beautiful序列的最小交换次数。

**思路**

有序数组就是beautiful的，因此题目可以理解为：求将数组升序排序或降序排序的最小交换次数。

类似于快速排序里面的partition部分，每次可以确定一个数的在最终位置，然后将这个数和对应位置上的数交换。先将数组进行一次排序（升序和降序分别进行）可以知道每个数最终的位置。

整体思路是贪心算法。

```python
def lilysHomework(arr):
    ascend = arr[:]
    descend = arr[:]
    ascend.sort()
    descend.sort(reverse=True)
    return min(changeTo(arr[:],ascend),changeTo(arr[:],descend))

def changeTo(originalArr,targetArr):
    '''
    minimum cost of changing originalArr to targetArr
    '''
    n = len(originalArr)
    positions = {originalArr[i]:i for i in range(n)}
    nmove = 0
    for i in range(n):
        if originalArr[i]!=targetArr[i]:
            # exchange arr[i] and the number which is supposed to be at arr[i]
            t1 = originalArr[i]
            originalArr[i] = originalArr[positions[targetArr[i]]]
            originalArr[positions[targetArr[i]]] = t1
            # alter their indices
            positions[t1] = positions[originalArr[i]]
            positions[originalArr[i]] = i
            nmove += 1
    return nmove
        
```



