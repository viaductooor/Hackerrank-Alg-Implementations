# Big Sorting

<https://www.hackerrank.com/challenges/big-sorting/problem>

给定一些表示数字的字符串（长度最多可以是![](https://latex.codecogs.com/gif.latex?10^6))，将他们排序输出。

**思路**

使用Python3实现排序的时候可以设置多个排序标准。一般参数key是一个返回单个值的函数，比如`lambda person:person.id`，但是如果有多个排序标准的话可以按照主次顺序把他们放在元组里面返回，比如`lambda person:(person.id,person.name,person.age)`。

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

