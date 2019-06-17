# Super Reduced String

https://www.hackerrank.com/challenges/reduced-string/problem

给定一个仅包含小写字母的字符串。如果存在两个字母相邻且相同，则从字符串中将他们删除，重复这一操作直到不能继续删除字母。求删除过后的字符串。

**思路 1**

字符串长度最多为100，因此不用太在意复杂度的问题。可以按照题目指定的步骤，分成多次删除，直到删除前后字符串长度相等。每轮删除的时候使用一个指针，开始指向第一个字母，如果与后面的字母相同则前进两位；否则添加当前字母到输出的字符串，前进一位。

```python
def superReducedString(s):
    s_before = s
    while True:
        s_after = reduce(s_before)
        if len(s_after)==len(s_before):
            if len(s_after)==0:
                return 'Empty String'
            else:
                return s_after
        s_before = s_after

def reduce(s):
    if len(s)<2:
        return s
    else:
        res = []
        p = 0
        while p<len(s)-1:
            if s[p]==s[p+1]:
                p += 2
            else:
                res.append(s[p])
                p += 1
            if p == len(s)-1:
                res.append(s[p])
    return ''.join(res)
```

**思路 2**

因为是顺序处理字符串，并且判断的是相邻两个字母的情况，所以可以考虑使用栈来处理。先将第一个字母入栈，从第二个字母开始遍历，如果当前字母和栈顶的字母相同，则将栈顶的字母出栈，否则当前字母入栈。

```python
def superReducedString(s):
    if len(s)<2:
        return s
    else:
        l = []
        for letter in s:
            if len(l)==0:
                l.append(letter)
            else:
                if l[-1]==letter:
                    l.pop()
                else:
                    l.append(letter)
        if len(l)==0:
            return 'Empty String'
        else:
            return ''.join(l)
```

# CamelCase

https://www.hackerrank.com/challenges/camelcase/problem

给定一个驼峰式命名的字符串，求该字符串包含的单次数量

**思路**

数大写字母个数，然后加1。

```py
def camelcase(s):
    numCap = 0
    for ch in s:
        if ch>='A' and ch<= 'Z':
            numCap += 1
    return numCap+1
```

# Strong Password

https://www.hackerrank.com/challenges/strong-password/problem

一个有效密码的要求有：

- 长度至少为6
- 包含至少一个数字
- 包含至少一个小写字母
- 包含至少一个大写字母
- 至少包含`!@#$%^&*()+-`中的一个

输入一个字符串，判断至少需要添加多少个字符才能使该字符串为有效密码。

**思路**

先统计每一种字符的个数，然后决定需要添加哪些。

```py
def minimumNumber(n, password):
    bucket = [0]*4
    special = '!@#$%^&*()-+'
    toadd = 0
    for ch in password:
        if ch.isdigit():
            bucket[0] += 1
        elif ch>='A' and ch<='Z':
            bucket[1] += 1
        elif ch>='a' and ch<='z':
            bucket[2] += 1
        elif ch in special:
            bucket[3] += 1
    for i in bucket:
        if i==0:
            toadd += 1
    if n+toadd<6:
        return 6-n
    else:
        return toadd
```

# Two Characters

https://www.hackerrank.com/challenges/two-characters/problem

给定一个字符串，消除其中的一些字符使其变成两个字符交替出现的字符串（比如"abab"）。每次删除一个字符会将字符串中跟他相同的字符一并删除。

输出删除后最长的交替字符组成的字符串的 **长度** 。

**思路**

最后得到的字符串仅包含两种字符，而他们在之前都是不会被删除的，所以在初始字符串中他们就是交替出现的（仅考虑这两种字符）。

给定的字符串只包含小写字母，所以交替出现的两个字符的组合其实非常有限。可以先统计每个字母出现的频率，因为最终得到的字符串中两种字符的出现频率差一定是小于1的，可以根据这个筛除掉很多组合。然后剩下的再去判断是否在原字符串中交替出现。

```py
def alternate(s):
    chs = {}
    alphas = 'abcdefghijklmnopqrstuvwxyz'
    maxlen = 0
    for ch in s:
        if ch in chs:
            chs[ch] += 1
        else:
            chs[ch] = 1
    for ch1 in alphas:
        for ch2 in alphas:
            if ch1!=ch2 and ch1 in chs and ch2 in chs:
                if chs[ch1]==chs[ch2]:
                    if qualify(s,ch1,ch2) or qualify(s,ch2,ch1):
                        maxlen = max(chs[ch1]*2,maxlen)
                elif chs[ch1]-chs[ch2]==1:
                    if qualify(s,ch1,ch2):
                        maxlen = max(chs[ch1]+chs[ch2],maxlen)
                elif chs[ch2]-chs[ch1]==1:
                    if qualify(s,ch2,ch1):
                        maxlen = max(chs[ch1]+chs[ch2],maxlen)
    return maxlen
                    

def qualify(s,ch1,ch2):
    '''
    判断ch1和ch2在s中是否是交替出现的
    '''
    l = []
    for ch in s:
        if ch==ch1:
            if len(l)==0:
                l.append(ch)
            elif l[-1]==ch2:
                l.append(ch)
            else:
                return False
        elif ch==ch2:
            if len(l)==0:
                return False
            elif l[-1]==ch1:
                l.append(ch2)
            else:
                return False
    return True
```

# Caesar Cipher

https://www.hackerrank.com/challenges/caesar-cipher-1/problem

定义一种加密方式：

- 字母右移k（大写字母只会转化成大写字母，小写字母只会转化为小写字母），比如a右移三位得到d，z右移三位得到c。
- 其他字母不变。

给定一个字符串，使用上面的方式加密输出加密字符串。

**思路**

循环移位K（shift）很多时候都可以通过每一位加K然后模序列长度来实现，包括数字的循环移位和题目中的字母的循环移位。

```py
def caesarCipher(s, k):
    uppers = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
    lowers = 'abcdefghijklmnopqrstuvwxyz'
    chl = []
    for ch in s:
        c = ch
        if ch >='A' and ch <= 'Z':
            c = uppers[(ord(ch)-ord('A')+k)//26]
        elif ch >='a' and ch <= 'z':
            ch = lowers[(ord(ch)-ord('a')+k)//26]
        chl.append(ch)
    return ''.join(chl)
```

# Mars Exploration

https://www.hackerrank.com/challenges/mars-exploration/problem

一个原始字符串S由若干个SOS组成，将其部分修改变成了S'，求修改了多少位。

**思路**

很容易知道每一位应该是什么字母，所以对比、计数即可。

```py
def marsExploration(s):
    res = 0
    sos = 'SOS'
    for i in range(len(s)):
        if s[i]!=sos[i%3]:
            res += 1
    return res
```

# HackerRank in a String!

https://www.hackerrank.com/challenges/hackerrank-in-a-string/problem

判断一个字符串是否包含值为'hackerrank'的子字符串（不要求连续）。

**思路**

先找到第一个字母'h'，然后用指针按序遍历后面的字符。

```py
def hackerrankInString(s):
    substr = 'hackerrank'
    if len(s)<len(substr):
        return 'NO'
    for h in range(len(s)-len(substr)+1):
        if s[h] == substr[0]:
            i = 1
            for p in range(h+1,len(s)):
                if s[p]==substr[i]:
                    i += 1
                    if i == len(substr):
                        return 'YES'
    return 'NO'
```
# Weighted Uniform Strings

https://www.hackerrank.com/challenges/weighted-uniform-string/problem

赋予每个字母权值，a~z分别是1~26，字符串的权值为每个组成字母权值的和。给定一个字符串，如果这个字符串是由单一字母组成的，那么这个字符串被称为uniform的。

给定一个字符串，把这个字符串所有uniform子字符串（在原字符串中连续）的权值放到集合U中。然后给定一些数字，判断他们是否属于U。

**思路**

先初始化一个空集合S。按顺序在初始字符串中找极大的连续子字符串，假设该字符串长度为n，且由权值为w的字符组成，那么就需要把`w, 2*w, 3*w,...,n*w`添加到S中。输入一个查询数字query，判断其是否在S中即可。

```py
def weightedUniformStrings(s, queries):
    letters = 'abcdefghijklmnopqrstuvwxyz'
    weights = {letters[i]:i+1 for i in range(26)}
    result = []
    allset = set()
    count = 1
    if len(s)<2:
        allset.add(weights[s[0]])
    else:
        for i in range(len(s)-1):
            if s[i]!=s[i+1]:
                for t in range(1,count+1):
                    allset.add(t*weights[s[i]])
                count = 1
            else:
                count += 1
            if i==len(s)-2:
                if s[i]==s[i+1]:
                    for t in range(1,count+2):
                        allset.add(t*weights[s[i]])
                else:
                    allset.add(weights[s[i+1]])
    for q in queries:
        if q in allset:
            result.append('Yes')
        else:
            result.append('No')
    return result
```

# 

# Separate the Numbers

https://www.hackerrank.com/challenges/separate-the-numbers/problem

给定一个整数，将其分割成两个及以上其他的**正整数**，要求：

- 后一个数比前一个数多1
- 每个分割后的数不能以0开头
- 不能打乱每个数字在原数中的顺序

比如1234可以分割成1，2，3和4。

**思路**

第一个数的取值至关重要。如果给定整数长度为n的话第一个数长度的可取值范围为[1,n/2]，遍历所有可能的长度来得到结果。确定一个有效数之后，下一个数的长度等于这个数的长度或者比这个数长度大1，很容易判断是继续向后查找还是结束这次循环。

```py
def separateNumbers(s):
    if s[0]=='0':
        return 'NO'
    n = len(s)
    for firstLen in range(1,n//2+1):
        number = int(s[0:firstLen])
        length = firstLen
        i = firstLen
        while True:
            if i == n:
                return 'YES '+s[0:firstLen]
            elif n-i<length:
                break
            elif n-i==length:
                if int(s[i:n])-number==1:
                    number = int(s[i:n])
                    i += length
                else:
                    break
            else:
                if int(s[i:i+length])-number==1:
                    number = int(s[i:i+length])
                    i += length
                elif int(s[i:i+length+1])-number==1:
                    number = int(s[i:i+length+1])
                    length += 1
                    i += length
                else:
                    break
    return 'NO'

```

# Pangrams

给定一个字符串，判断是否26个英文字母都至少在字符串中出现了一次。

**思路**

使用集合。

```python
def pangrams(s):
    letters = set(list('abcdefghijklmnopqrstuvwxyz'))
    lowers = s.lower()
    for ch in lowers:
        if ch in letters:
            letters.remove(ch)   
            if len(letters)<1:
                return 'pangram'
    return 'not pangram'
```

# Funny String

<https://www.hackerrank.com/challenges/funny-string/problem>

给定一个字符串（长度为n），计算它所有相邻两个字符的ASCII码的差的绝对值，得到长度为n-1的数组B。同时对A的逆序数组执行这个操作，得到B'。判断B和B'是否相等。

**思路**

先求出B，然后判读B是不是回文。

```python
def funnyString(s):
    arr = []
    for i in range(len(s)-1):
        arr.append(abs(ord(s[i])-ord(s[i+1]))
    for i in range(len(arr)//2):
        if arr[i]!=arr[len(arr)-1-i]:
        return 'Not Funny'
    return 'Funny'
```

# Gemstones

给定一些由小写字母字符串，求这些字符串都包含的字母的个数。

**思路**

用集合的交运算。

```python
def gemstones(arr):
    letters = set(list('abcdefghijklmnopqrstuvwxyz'))
    for s in arr:
        letters &= set(list(s))
    return len(letters)
```

# Alternative Characters

给定一个仅由“A”和“B”组成的字符串，删除其中的一些字符使其变为A和B交替出现的字符串，求最少需要删除的字符的个数。

**思路**

假如某个字符连续出现了k次（k>1），那么至少需要删除k-1个数。解题的关键在于对连续字符的计数，可以考虑用栈实现。

```python
def alternatingCharacters(s):
    stack = []
    total = 0
    for ch in s:
        if len(stack)==0:
            stack.append(ch)
        else:
            if ch == stack[-1]:
                stack.append(ch)
            else:
                total += len(stack)-1
                stack.clear()
                stack.append(ch)
    total += len(stack)-1
    return total
```

# Beautiful Binary String

<https://www.hackerrank.com/challenges/beautiful-binary-string/problem>

如果一个二进制字符串（只包含1和0的字符串）中不存在'010'这样的序列，则这个字符串是beautiful的。给定一个二进制字符串，每次修改其中的一位（0变成1或者1变成0），求将其变为beautiful的最少的修改次数。

**思路**

遍历字符串中的字符，找到所有“010”的子字符串并记录下他们的位置。两个“010”之间只能间隔1个字符（01010）或者2个及以上个字符（010010）。第二种情况应该直接分开计算，有多少个010就需要修改多少次；第一种情况比较特殊，一个01010只需要修改一次（将中间的0改为1）。