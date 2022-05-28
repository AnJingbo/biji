

![题目](https://img-blog.csdnimg.cn/20210404162538983.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
**1、确定dp数组及其下标的含义**

dp[i][j]：有k枚鸡蛋，n层楼，确定f的确切值的最小操作次数

**2、确定递推公式**

一个鸡蛋放在第 x 层扔下去，有两种情况：

（1）如果鸡蛋碎了，那么鸡蛋的个数应该减一，同时说明再往上的楼层都不行了，搜索的楼层区间应该从变为 [1 ... x-1] 共 x - 1层楼，结果是 dp[i - 1][x - 1] + 1。

（2）鸡蛋没碎，那么鸡蛋的个数不变，同时说明再往下的楼层都可以，搜索的楼层区间应该变为 [x+1 ... j] 共j - x层楼，结果是 dp[i][j - x] + 1。

**3、如何初始化**

如果 i 等于1，说明只有一个鸡蛋，那么需要一层一层地去试，因此操作次数就是楼层高度 j

如果 j 等于1，说明只有一层楼高，那么只需要扔一次就可以确认f，因此操作次数是1；

其余部分都设置为最大值，因为后面需要求Math.min

以下代码会超时：
```java
class Solution {
    public int superEggDrop(int k, int n) {
        int[][] dp = new int[k + 1][n + 1];// 有k枚鸡蛋，n层楼，确定f的确切值的最小操作次数
        for(int i = 1; i <= k; i++){// 只有一层楼高
            dp[i][1] = 1;
        }
        for(int j = 1; j <= n; j++){// 只有一个鸡蛋，需要一层一层的试
            dp[1][j] = j;
        }
        for(int i = 2; i <= k; i++){
            for(int j = 2; j <= n; j++){
                dp[i][j] = Integer.MAX_VALUE;
            }
        }
        for(int i = 2; i <= k; i++){
            for(int j = 2; j <= n; j++){
                for(int x = 1; x <= j; x++){
                    dp[i][j] = Math.min(dp[i][j], Math.max(dp[i - 1][x - 1], dp[i][j - x]) + 1);
                }
            } 
        }
        return dp[k][n];
    }
}
```
记忆化递归（超时）：
```java
class Solution {
    int[][] memo;
    public int superEggDrop(int k, int n) {
        memo = new int[k + 1][n + 1];
        for(int i = 1; i <= k; i++){
            for(int j = 1; j <= n; j++){
                memo[i][j] = -1;
            }
        }
        return dp(k, n);
    }
    public int dp(int k, int n){// 有k枚鸡蛋，n层楼，确定f的确切值的最小操作次数
        if(k == 1){// 如果只有1个鸡蛋，只能一层一层往上试
            return n;
        }
        if(n == 1){// 如果楼层数为1，只需要扔一次鸡蛋
            return 1;
        }
        if(memo[k][n] != -1){// 避免重复计算
            return memo[k][n];
        }
        int res = Integer.MAX_VALUE;
        for(int i = 1; i <= n; i++){
            res = Math.min(res, Math.max(dp(k, n - i), dp(k - 1, i - 1)) + 1);
        }
        memo[k][n] = res;// 记入备忘录
        return res;
    }
}
```