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
- i不为0，则A[i]=A[i-1]⊕i，⊕表示异或

求A中任意一个区间A[l]到A[r]的所有数的异或和。

## 思路

根据推导，A[i]=1⊕2⊕3⊕...⊕i

令B[i]=A[0]⊕A[1]⊕A[2]⊕...⊕A[i]，则推导开来得到，B[i]=

- 0⊕2⊕...⊕i，如果i为偶数
- 1⊕3⊕...⊕i，如果i为奇数

A[l]⊕A[l+1]⊕...⊕A[r-1]⊕A[r] = B[r]⊕B[l-1]

**奇数（偶数）的异或和**

下面的问题是如何解决0⊕2⊕...⊕i以及1⊕3⊕...⊕i的问题。偶数数列其实是1,2,...i/2分别左移一位，所以可以先求1⊕2⊕3⊕...⊕i/2然后整体左移一位得到结果；奇数和偶数的区别是末尾位为1，所以可以先求2⊕4⊕...⊕i-1，然后根据序列的长度来决定是加1。

现在的问题是如何求0⊕1⊕2⊕3⊕...⊕i。

0⊕1⊕2⊕3⊕...⊕i的规律是每4个异或和为0。假如有一个数K，左移两位（等同于乘以4）之后为K00，那么分别加上1、2和3后得到K01、K10、K11，这4个数亦或得到0。也就是说，**4K⊕(4K+1)⊕(4K+2)⊕(4K+3)=0，k=0,1,2,...** 。

```python
def xorSequence(l, r):
    return xor(r)^xor(l-1)
    
def allXor(n):
    k,r = int(n//4),int(n%4)
    ex = [0,1,3,0]
    ks = [k<<2,0,k<<2,0]
    return ks[r]+ex[r]

def xor(n):
    if n%2==0:
        return allXor(n//2)<<1
    else:
        num = (n-1)//2+1
        return (allXor((n-1)//2)<<1)^(num%2)
```

# Sum vs XOR

https://www.hackerrank.com/challenges/sum-vs-xor/problem

给定一个n（0~10^15），找到所有的x（0~n），使n+x=n⊕x。输出x可能的个数。

## 思路

异或可以看作不进位的加法，所以只要保证加法过程中不进位，那么和异或的结果就是相同的。

先把n用二进制表示，n中为1的位在x中只能为0，n中为0的位在x中可以为1或0。因此先计算n中0的个数，2的n次幂就是x的可能的个数。

```python
def sumXor(n):
    numZero=0
    while n>0:
        numZero+=(1-n%2)
        n>>=1
    return 2**numZero
```

# The Great XOR

https://www.hackerrank.com/challenges/the-great-xor/problem

给定一个长整型数x，找到满足下面条件的a的 **数量** ：

- a⊕x > x
- 0 < a < x

## 思路

如果修改x中的一些位，使其值增加，那么一定有一位是从0修改到1。从高到低找到第一个这样的位，记为e。e的特点有：

- e可以是x中任意值为0的位
- 将e置为1之后，e之后的所有位的值可以随意修改、组合，其值一定大于x
- 所有这样的修改都可以通过异或上一个比x小的数实现，也即是a。

解题思路就是找到x中所有值为0的位，并记录下他们的位置，然后转化为组合问题求解。

```python
def theGreatXor(x):
    zeros = []
    count = 0
    while x>0:
        r = x%2
        x >>= 1
        if r==0:
            zeros.append(count)
        count += 1
    if len(zeros)==0:
        return 0
    else:
        return sum([2**i for i in zeros])
```

# Flipping bits

https://www.hackerrank.com/challenges/flipping-bits/problem

给定一个32位无符号整型的数，将其转化位二进制数之后，每一位都翻转一下，输出翻转之后的无符号整型数。

# 思路

Python的数number是没有大小限制的，所以不用考虑溢出的问题。直接用这个数异或全1（(1<<32)-1）即可。

需要注意的是，位操作符的优先度是低于加减乘除的，所以要加括号。

```python
def flippingBits(n):
    return n^((1<<32)-1)
```

# Yet Another Minimax Problem

https://www.hackerrank.com/challenges/yet-another-minimax-problem/problem


给定一组数，对于这组数的任意一个序列a0,a1,a2,...an，计算：

b0 = a0⊕a1
b1 = a1⊕a2
...
bn-1 = an-1⊕an

最大的b的值就是这个序列的分数。找到分数最小的序列，输出这个序列的分数。

## 思路

