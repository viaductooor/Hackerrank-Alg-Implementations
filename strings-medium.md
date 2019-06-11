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