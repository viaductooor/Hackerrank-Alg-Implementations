# Sherlock and the Valid String

https://www.hackerrank.com/challenges/sherlock-and-valid-string/problem

如果一个字符串中所有字符出现的频率相等，或者删除掉一个字符（仅一个）使得其他字符出现频率相等，则这个字符串是valid的。

给定一个字符串，判断是否为valid。

**思路**

先计算每个字符出现的频率，出现以下情况则可以判断是valid的：

1. 所有字符出现频率相同
2. 除了一个字符出现次数为1，其他字符出现次数相同
3. 除了一个字符以外，其他字符出现次数相同，并且这个字符出现的次数仅比其他字符多1

解决问题的关键在于先对所有字符计数，然后对他们出现的频率计数。

```py
def isValid(s):
    chs = statistic(s)
    if len(chs) < 2 or len(s)<4:
        # case 1
        return 'YES'
    else:
        f = chs.values()
        sta = statistic(f)
        if len(sta) == 1:
            return 'YES'
        elif len(sta) > 2:
            return 'NO'
        else:
            keys = list(sta.keys())
            bigger,smaller = max(keys),min(keys)
            if smaller==1 and sta[smaller]==1:
                # case 2
                return 'YES'
            else:
                if bigger-smaller==1 and sta[bigger]==1:
                    # case 3
                    return 'YES'
                return 'NO'
            
def statistic(l):
    d = {}
    for i in l:
        if i in d:
            d[i] += 1
        else:
            d[i] = 1
    return d 

```

# Highest Value Palindrome

https://www.hackerrank.com/challenges/richie-rich/problem

给定一个由n个数字组成的字符串，要求修改其中的几位（至多k位），使其变成回文。求变成回文后表示的数字的值最大的字符串。如果不能转化为回文则返回-1。

**思路**

回文字符串对称位置的字符相等，可以遍历左边（或右边）一半看对应位置字符是否相等，如果不相等则取两者中的较大值，记录下所有改动的位置。这样一来，如果改动次数不足的话就直接返回-1；如果改动次数正好用完的话则直接把改动后的字符串返回；如果改动次数有剩余的话，则应该尽可能地从高位开始把部分字符改为9。

需要注意的地方有：

- 改为9的时候，如果之前改过这个数，现在只需要增加一次修改就可以了；如果没有改过，则需要改两次。
- 改为9之前要判断是否为9，以及是否有足够的修改次数。
- 如果字符串长度为奇数，需要单独考虑中间的数。
- 最后返回的数字是不包含高位的0的，也就是说字符串'0990'最后返回为'990'。

```py
def highestValuePalindrome(s, n, k):
    op = k
    l = list(s)
    alterList = []
    for i in range(n//2):
        if l[i]!=l[n-1-i]:
            bigger = max(l[i],l[n-1-i])
            l[i],l[n-1-i] = bigger,bigger
            alterList.append(i)
            op -= 1
            if op < 0:
                return '-1'
    if op > 0:
        for i in range(n//2):
        	if op < 1:
            	break
            if l[i]!='9':
            	if i in alterList:
            		l[i],l[n-1-i] = '9','9'
            		op -= 1
            	elif op > 1:
            		l[i],l[n-1-i] = '9','9'
            		op -= 2
    if n%2!=0 and l[n//2]!='9' and op>0:
        l[n//2] = '9'        
    return str(int(''.join(l)))
```

# Maximum Palindromes

https://www.hackerrank.com/challenges/maximum-palindromes/problem

给定一个字符串s，选择s的一个子字符串（从left到right，1<=left<=right<=|s|），将其重新组合（可选择部分）形成回文字符串，求长度最长的回文字符串有多少种。

**思路**

回文字符串中最多只有一个字符出现奇数次，其他字符出现偶数次。

# Sherlock and Anagrams

如果两个字符串包含的字符种类及数目相同，则他们是**无序变形字符串对**。给定一个字符串，求它的无序变形子字符串对的数量。

字符顺序可以相同，但是必须是两个不同的子字符串。

**思路**

只有长度相同的字符串才可能符合要求，所以可以根据长度来遍历所有可能的情况。

```py
def sherlockAndAnagrams(s):
	total = 0
	for size in range(1,len(s)):
		for i in range(len(s)-size):
			for j in range(i+1,len(s)-size+1):
				s1 = s[i:i+size]
				s2 = s[j:j+size]
				if isAna(s1,s2):
					total += 1
	return total
def isAna(s1,s2):
	l1,l2 = list(s1),list(s2)
	l1.sort()
	l2.sort()
	for i in range(len(s1)):
		if l1[i]!=l2[i]:
			return False
	return True
```

# Common Child

如果字符串A可以通过删除0个或多个字符，从而形成字符串B，那么就成B为A的子串（child）。给定两个字符串，求他们最长公共子串的长度。

**思路**

动态规划

