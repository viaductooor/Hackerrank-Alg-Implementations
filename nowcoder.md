# 矩阵的最小路径和

[Code on Nowcoder](https://www.nowcoder.com/practice/2fb62a4500af4f4ba5686c891eaad4a9)

动态规划，从右下角向左上角求解，`dp[i][j]`表示从左上角到`matrix[i][j]`的最小代价（%65 AC，超时）：

```python
def getInput():
    n,m = [int(s) for s in input().strip().split(' ')]
    matrix = [None for _ in range(n)]
    for i in range(n):
        matrix[i] = [int(s) for s in input().strip().split(' ')]
    return n,m,matrix

def resolve(n,m,matrix):
    dp = [[0 for _ in range(m)] for _ in range(n)]
    dp[n-1][m-1] = matrix[n-1][m-1]
    for i in range(m-2,-1,-1):
        dp[n-1][i] = matrix[n-1][i] + dp[n-1][i+1]
    for i in range(n-2,-1,-1):
        dp[i][m-1] = matrix[i][m-1] + dp[i+1][m-1]
    for i in range(n-2,-1,-1):javascript:void(0);
        for j in range(m-2,-1,-1):
            dp[i][j] = matrix[i][j]+min(dp[i+1][j],dp[i][j+1])
    return dp[0][0]

n,m,matrix = getInput()
print(resolve(n,m,matrix))
```

动态规划，从左上角到右下角，只使用一行数据（一维dp）来记录，dp[i]表示从`matrix[0][0]`开始到当前行的第i个格子的最少代价，最后输出dp[m-1]（95% AC，仍然超时）。

```python
def getRow():
    return [int(s) for s in input().strip().split(' ')]

def resolve():
    n,m = getRow()
    dp = [0 for _ in range(m)]
    for i in range(n):
        nums = getRow()
        if i==0:
            dp[0] = nums[0]
            for j in range(1,m):
                dp[j] = nums[j]+dp[j-1]
        else:
            dp[0] = dp[0] + nums[0]
            for j in range(1,m):
                dp[j] = nums[j]+min(dp[j],dp[j-1])
    return dp[-1]
print(resolve())
```

同样思路的Java代码实现（100% AC）：

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        try(Scanner sc = new Scanner(System.in)){
            int n = sc.nextInt();
            int m = sc.nextInt();
            int[] dp = new int[m];
            for(int i=0;i<n;i++){
                int[] row = new int[m];
                for(int j=0;j<m;j++){
                    row[j] = sc.nextInt();
                }
                if(i==0){
                    dp[0] = row[0];
                    for(int j=1;j<m;j++){
                        dp[j] = row[j]+dp[j-1];
                    }
                }else{
                    dp[0] = row[0] + dp[0];
                    for(int j=1;j<m;j++){
                        dp[j] = row[j] + (dp[j-1]<dp[j]?dp[j-1]:dp[j]);
                    }
                }
            }
            System.out.print(dp[m-1]);
        }
    }
}
```

# 机器人到达指定位置方法数

[Code on Nowcoder](https://www.nowcoder.com/practice/54679e44604f44d48d1bcadb1fe6eb61)

动态规划，用一维数组表示dp，`dp[i]`表示机器人到位置i的方案数。一开始除了`dp[m]=1`以外其他位置都是0，表示可移动0次的情况下到各个位置的方案数。之后循环更新k次，每次得到的结果都是根据第k-1次推导得来的。

需要注意的是要用到k-1次的部分数据，所以可以直接用一个数组将前一次的结果保存下来，或者是看需要第k-1次的几个值，就用几个中间变量来记录他们。

```java
import java.util.Scanner;
import java.lang.Math;

public class Main{
    public static void main(String[] args){
        int MOD = 1000000007;
        int n,m,k,p;
        try(Scanner sc = new Scanner(System.in)){
            n = sc.nextInt();
            m = sc.nextInt()-1;
            k = sc.nextInt();
            p = sc.nextInt()-1;
        }
        int[] dp = new int[n];
        dp[m] = 1;
        for(int i=0;i<k;i++){
            int left = 0;
            for(int j=0;j<n;j++){
                if(j==0){
                    left = dp[j];
                    dp[j] = dp[j+1];
                }else if(j==n-1){
                    dp[j] = left;
                }else{
                    int t = left;
                    left = dp[j];
                    dp[j] = (t+dp[j+1]) % MOD;
                }
            }
        }
        System.out.println(dp[p]);
    }
}
```