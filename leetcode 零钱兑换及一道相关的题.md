

![ts](https://img-blog.csdnimg.cn/d3f1a2dd3cfa43d7a3ae4e253cc9125b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public int change(int amount, int[] coins) {
        int[][] dp = new int[coins.length + 1][amount + 1]; // 从前 i 个硬币任意取，能够凑成总金额 j 的组合数
        dp[0][0] = 1; // 当没有任何硬币可以选取时，总金额只能是 0，对应的方案数是 1，
        for(int i = 1; i <= coins.length; i++){
            for(int j = 0; j <= amount; j++){
                if(j >= coins[i - 1]){
                    dp[i][j] = dp[i - 1][j] + dp[i][j - coins[i - 1]];
                }else{
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        return dp[coins.length][amount];
    }
}
```
![ts](https://img-blog.csdnimg.cn/8077b4824ec74de5bf69e7ccebc50c44.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[][] dp = new int[coins.length + 1][amount + 1]; // 从前 i 个硬币随便取，能够凑成总金额 j 的最小硬币数
        for(int j = 1; j <= amount; j++){
            dp[0][j] = Integer.MAX_VALUE; 
        }
        for(int i = 1; i <= coins.length; i++){
            for(int j = 1; j <= amount; j++){
                if(j >= coins[i - 1] && dp[i][j - coins[i - 1]] != Integer.MAX_VALUE){
                    dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - coins[i - 1]] + 1);
                }else{
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        return dp[coins.length][amount] == Integer.MAX_VALUE ? -1 : dp[coins.length][amount];
    }
}
```
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1]; // 凑足金额 i 所需的最少硬币个数是 dp[i]
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;
        for(int i = 1; i <= amount; i++){
            for(int j = 0; j < coins.length; j++){
                if(i >= coins[j] && dp[i - coins[j]] != Integer.MAX_VALUE){
                    dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
                }
            }
        }
        return dp[amount] == Integer.MAX_VALUE ? -1 : dp[amount];
    }
}
```
![ts](https://img-blog.csdnimg.cn/249c90a74a0d474d87788e019f2aea16.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1]; // 和为 i 的完全平方数的最小数量是 dp[i]
        for(int i = 1; i <= n; i++){
            dp[i] = i; // 最坏的情况就是每次+1 比如: dp[3]=1+1+1
            for(int j = 1; j * j <= i; j++){
                dp[i] = Math.min(dp[i], dp[i - j * j] + 1);
            }
        }
        
        return dp[n];
    }
}
```