# Roads and Libraries

https://www.hackerrank.com/challenges/torque-and-development/problem

给定一张包含多个城市的图，城市之间有路相连。现在所有的路都被破坏，需要选择部分修理，同时需要选择几个城市建立图书馆。修路的费用是c_road，建立图书馆的费用是c_lib，要求：

- 所有城市都可以到达图书馆
- 总修建费用最低

**思路**

因为不一定所有城市都是连通的，所以需要先分别处理各个极大连通子图，每个极大连通子图至少需要一个图书馆。

先求出来各个极大连通子图的生成树，如果只设置一个图书馆的话，总费用为c_lib+c_road*(n-1)，其中n为这个连通片中顶点的数量。显然，多修一个图书馆的同时可以少修一条路，也就是把这个生成树分割成两个生成树。如果修建图书馆的费用更高，就应该每个极大连通子图只修一个图书馆；否则，应该每个城市都修一个图书馆。

如果是第二种情况，根据推导，只要确定了极大连通子图的个数为K，那么总费用就是：

`N*c_road + K(c_lib-c_road)`

其中N是城市总个数。

```python
def roadsAndLibraries(n, c_lib, c_road, cities):
    visited = set()
    if c_lib <= c_road:# or len(cities)<1:
        return c_lib * n
    else:
        ajl = formAdjlist(cities)
        numpatches = 0
        for c in ajl.keys():
            if c not in visited:
                visit(c,ajl,visited)
                numpatches += 1
        # some single-vertex sub-graphs that are not included in cities
        numpatches += n - len(visited)
        return n * c_road + numpatches * (c_lib - c_road)

def formAdjlist(cities):
    # 生成邻接表
    ajl = {}
    for city in cities:
        if city[0] in ajl:
            ajl[city[0]].append(city[1])
        else:
            ajl[city[0]] = [city[1]]
        if city[1] in ajl:
            ajl[city[1]].append(city[0])
        else:
            ajl[city[1]] = [city[0]]
    return ajl

def visit(city,adjlist,visited):
    # dfs递归访问一个顶点
    visited.add(city)
    for c in adjlist[city]:
        if c not in visited:
            visit(c,adjlist,visited)
```

# Journey to the Moon

https://www.hackerrank.com/challenges/journey-to-the-moon/problem

同一对的宇航员来自相同的国家，比如(1,2)表示宇航员1和宇航员2来自同一个国家；现在有多个这样的对。需要从所有宇航员中选择两个来自不同国家的，求有多少种选择方法。

**思路**

将所有宇航员视为顶点，如果两个宇航员来自相同的国家则他们之间有边相连。有多少个极大连通子图则有多少个国家，然后这个问题就完全变成了排列组合问题。

需要注意的是，不是所有宇航员都出现在了列表当中（表示一个国家仅有一个宇航员），所以要单独处理这些情况。

```python
def journeyToMoon(n, astronaut):
    visited = set()
    patches = []
    ajl = formAdjlist(astronaut)
    for c in ajl.keys():
        if c not in visited:
            before_visit = len(visited)
            visit(c, ajl, visited)
            after_visit = len(visited)
            patches.append(after_visit - before_visit)
    total = 0
    # 处理各个极大连通子图（不包含平凡图）
    for i in range(len(patches) - 1):
        for j in range(i+1,len(patches)):
            total += patches[i] * patches[j]
    # 处理极大的平凡子图
    numsole = n - len(visited)
    if numsole > 0:
        total += numsole * (numsole - 1) // 2
    for p in patches:
        total += p*numsole
    return total


def formAdjlist(cities):
    # 生成邻接表
    ajl = {}
    for city in cities:
        if city[0] in ajl:
            ajl[city[0]].append(city[1])
        else:
            ajl[city[0]] = [city[1]]
        if city[1] in ajl:
            ajl[city[1]].append(city[0])
        else:
            ajl[city[1]] = [city[0]]
    return ajl


def visit(city, adjlist, visited):
    # dfs递归访问一个顶点
    visited.add(city)
    for c in adjlist[city]:
        if c not in visited:
            visit(c, adjlist, visited)
```

上面的代码有一个测试用例通过不了，原因是递归调用的深度超过限制。

