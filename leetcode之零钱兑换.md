

322. 零钱兑换
给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

你可以认为每种硬币的数量是无限的。

示例 1：
输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1
示例 2：
输入：coins = [2], amount = 3
输出：-1

题目中说每种硬币的数量是无限的，可以看出是典型的完全背包问题。


动规五部曲：

**1、确定dp数组以及下标的含义**

dp[j]：**凑足总额为j所需钱币的最少个数为dp[j]**

**2、确定递推公式**

**如果你不把这第i个物品装入背包**，也就是说你不使用coins[i]这个面值的硬币，那么就继承之前的结果。

**如果你把这第i个物品装入了背包**，也就是说你使用coins[i]这个面值的硬币，那么dp[j]应该等于dp[j-coins[i]] + 1。

即凑足总额为j - coins[i]的最少硬币个数是dp[j - coins[i]]，那么只需要加上一个钱币coins[i]即dp[j - coins[i]] + 1就是dp[j]。

所以dp[j] 要取所有 dp[j - coins[i]] + 1 中最小的。

递推公式：**dp[j] =  min(dp[j - coins[i]] + 1, dp[j]);**

**3、dp数组如何初始化**

首先**凑足总金额为0所需钱币的个数一定是0，那么dp[0] = 0;**

其他下标对应的数值呢？

**考虑到递推公式的特性，dp[j]必须初始化为一个最大的数，否则就会在min(dp[j - coins[i]] + 1, dp[j])比较的过程中被初始值覆盖。**

所以下标非0的元素都是应该是最大值。

**4、确定遍历顺序**

我们知道这是完全背包，

如果求组合数就是外层for循环遍历物品，内层for循环遍历背包；

如果求排列数就是外层for循环遍历背包，内层for循环遍历物品；

但本题是求钱币最小个数，那么钱币有顺序和没有顺序都可以，都不影响钱币的最小个数。

所以**本题并不强调集合是组合还是排列**。

如果求组合数就是外层for循环遍历物品，内层for遍历背包。

如果求排列数就是外层for遍历背包，内层for循环遍历物品。

所以本题的两个for循环的关系是：外层for循环遍历物品，内层for遍历背包或者外层for遍历背包，内层for循环遍历物品都是可以的！

本题钱币数量可以无限使用，那么是完全背包。所以遍历的内循环是正序

**5、举例推导dp数组**
以输入：coins = [1, 2, 5], amount = 5为例
![图示](https://img-blog.csdnimg.cn/20210203104114440.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];//盛满容量为 j 的背包所需最小硬币数是dp[j]
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;
        for(int i = 0; i < coins.length; i++){//遍历物品
            for(int j = 1; j <= amount; j++){//遍历背包
                if(j >= coins[i] && dp[j - coins[i]] != Integer.MAX_VALUE){//如果dp[j - coins[i]] == Integer.MAX_VALUE说明没有任何硬币能够组成该金额
                    dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1);
                }
            }
        }
        return dp[amount] == Integer.MAX_VALUE ? -1 : dp[amount];
    }
}
```
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];//盛满容量为 j 的背包所需最小硬币数是dp[j]
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;
        for(int j = 1; j <= amount; j++){//遍历背包
            for(int i = 0; i < coins.length; i++){//遍历物品
                if(j >= coins[i] && dp[j - coins[i]] != Integer.MAX_VALUE){
                    dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1);
                }
            }
        }
        return dp[amount] == Integer.MAX_VALUE ? -1 : dp[amount];
    }
}
```