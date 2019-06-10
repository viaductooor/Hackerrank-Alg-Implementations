# Roads and Libraries

https://www.hackerrank.com/challenges/torque-and-development/problem

给定一张包含多个城市的图，城市之间有路相连。现在所有的路都被破坏，需要选择部分修理，同时需要选择几个城市建立图书馆。修路的费用是c_road，建立图书馆的费用是c_lib，要求：

- 所有城市都可以到达图书馆
- 总修建费用最低

**思路**

因为不一定所有城市都是连通的，所以需要先分别处理各个连通片，每个连通片至少需要一个图书馆。

先求出来各个连通片的生成树，如果只设置一个图书馆的话，总费用为c_lib+c_road*(n-1)，其中n为这个连通片中顶点的数量。显然，多修一个图书馆的同时可以少修一条路，也就是把这个连通片分割成两个连通片。如果修建图书馆的费用更高，就应该每个连通片只修一个图书馆；否则，应该每个城市都修一个图书馆。

如果是第二种情况，根据推导，只要确定了连通片的个数为K，那么总费用就是：

`N*c_road + K(c_lib-c_road)`

其中N是城市总个数。

```python
def roadsAndLibraries(n, c_lib, c_road, cities):
    if c_lib <= c_road:
        return c_lib * n
    else:
        visited = set()
        numpatches = 0
        for city in cities:
            if city[0] in visited:
                visited.add(city[1])
            elif city[1] in visited:
                visited.add(city[0])
            else:
                visited.add(city[0])
                visited.add(city[1])
                numpatches += 1
        numpatches += n - len(visited)
        return n * c_road + numpatches * (c_lib - c_road)
```