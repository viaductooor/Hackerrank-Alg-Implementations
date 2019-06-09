# Lonely Integer

给定一个数组，除了有一个数只出现一次以外，其余的数都出现两次。找出这个只出现一次的数。

```
Sample Input: 0 0 1 2 1
Sample Output: 2
```

## 思路1

可以用set来实现。遍历数组，如果这个数set中已经存在，则将其删除；否则将其添加到set，最后剩下的就是只出现一次的数。

```python
def lonelyinteger(a):
    s = set()
    for i in a:
        if i in s:
            s.remove(i)
        else:
            s.add(i)
    return s.pop()
```

## 思路2

使用位亦或实现。一个数和他本身亦或得到的是0，0和任何数亦或得到那个数本身，因此把这个数组中的所有数亦或一遍得到的数就是那个只出现一次的数。

```python
def lonelyinteger(a):
    result = 0
    for i in a:
        result ^= i
    return result
```

# Maximizing XOR

https://www.hackerrank.com/challenges/maximizing-xor/problem

给定两个整数：L 和 R

∀ L ≤ A ≤ B ≤ R, 找出 A xor B 的最大值。

## 思路1

蛮力法。因为问题规模在这里是1000，所以可以接受复杂度为O(n^ 2)的算法。

```python
def maximizingXor(l, r):
    maxxor = 0
    for i in range(l,r):
        for j in range(i+1,r+1):
            maxxor = max(maxxor,i^j)
    return maxxor
```

## 思路2

先将L和R亦或一下，记录下这个数的最高位。最后的结果就是将这一位以及之后所有的位置为1。

```python
def maximizingXor(l, r):
    v = l^r
    return int(math.pow(2,math.floor(math.log(v,2)+1))-1)
```

# Counter Game

https://www.hackerrank.com/challenges/counter-game/problem

将一个数N执行一系列操作：

- 如果N是2的幂，则将N减少一半
- 如果N不是2的幂，则将N减少小于N的最大2的幂
- 如果N为1，结束，返回这一系列操作的次数

## 思路

- 判断N是否为2的幂就是判断（N与N-1），值为0则为2的幂；
- 减少一半就是右移一位；
- “减少小于N的最大2的幂”就是将N的最高位置为0。

```python
def counterGame(n):
    count = 0
    while n!=1:
        if n & n-1:
            # circumstance 1
            k = math.floor(math.log(n,2))
            n = int(n - math.pow(2,k))
        else:
            # circumstance 2
            n = n >> 1
        count += 1
        print(n)

    if count%2!=0:
        return 'Louise'
    else:
        return 'Richard'
```

# Xor-sequence

https://www.hackerrank.com/challenges/xor-se/problem

A是一个序列，A中的元素A[i]的定义如下：

- i为0，则A[i]=0
- i不为0，则A[i]=A[i-1]^i，^表示异或

求A中任意一个区间A[l]到A[r]的所有数的异或和。

## 思路

根据推导，A[i]=1^2^3^...^i

令B[i]=A[0]^A[1]^A[2]^...^A[i]，则推导开来得到，B[i]=

- 0^2^...^i，如果i为偶数
- 1^3^...^i，如果i为奇数

A[l]^A[l+1]^...^A[r-1]^A[r] = B[r]^B[l-1]

**奇数（偶数）的异或和**


