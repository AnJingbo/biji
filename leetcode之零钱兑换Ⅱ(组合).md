

518. 零钱兑换 II
给定不同面额的硬币和一个总金额。写出函数来计算可以凑成总金额的硬币组合数。假设每一种面额的硬币有无限个。 
示例 1:
输入: amount = 5, coins = [1, 2, 5]
输出: 4
解释: 有四种方式可以凑成总金额:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
示例 2:
输入: amount = 3, coins = [2]
输出: 0
解释: 只用面额2的硬币不能凑成总金额3。
示例 3:
输入: amount = 10, coins = [10] 
输出: 1
注意:
你可以假设：
0 <= amount (总金额) <= 5000
1 <= coin (硬币面额) <= 5000
硬币种类不超过 500 种
结果符合 32 位符号整数

**注：一维dp在求装满背包有几种方案的时候，认清遍历顺序是很重要的：
如果求组合数就是外层for循环遍历物品，内层for循环遍历背包；
如果求排列数就是外层for循环遍历背包，内层for循环遍历物品。**
思路：
动规五部曲：
**1、确定dp数组以及下标含义**
dp[j]：凑成总金额为 j 的最大组合数是dp[j]

**2、确定递推公式**
dp[j] += dp[j - coins[i]];

**3、dp数组如何初始化**
dp[0]一定要为1，dp[0] = 1是递推公式的基础。
即 当总金额为 0 的时候的最大组合数是 1。
下标非0的dp[j]初始化为0，这样累加dp[j - coins[i]]的时候才不会影响真正的dp[j]。

**4、确定遍历顺序**
完全背包的两个for循环先后顺序无所谓，但本题就不行了，本题需要先外层遍历物品（钱币），内层遍历背包（金钱总额）。

外层for循环遍历物品（钱币），内层for遍历背包（金钱总额）的情况。
代码如下：
```java
for (int i = 0; i < coins.size(); i++) { // 遍历物品
    for (int j = coins[i]; j <= amount; j++) { // 遍历背包容量
        if(j >= coins[i]){
            dp[j] += dp[j - coins[i]];
        }
    }
}
```
假设：coins[0] = 1，coins[1] = 5。

那么就是先把1加入计算，然后再把5加入计算，得到的方法数量只有{1, 5}这种情况。而不会出现{5, 1}的情况。因为coins遍历放在外层，5只能出现在1的后面！
所以这种遍历顺序中dp[j]里计算的是组合数！

如果把两个for交换顺序，代码如下：
```java
for (int j = 0; j <= amount; j++) { // 遍历背包容量
    for (int i = 0; i < coins.size(); i++) { // 遍历物品
        if (j - coins[i] >= 0){
            dp[j] += dp[j - coins[i]];
        }
    }
}
```

背包容量的每一个值，都是经过 1 和 5 的计算，包含了{1, 5} 和 {5, 1}两种情况。
此时dp[j]里算出来的就是排列数！

可能这里很多同学还不是很理解，建议动手把这两种方案的dp数组数值变化打印出来，对比看一看！（实践出真知）
**注：一维dp在求装满背包有几种方案的时候，认清遍历顺序是很重要的：
如果求组合数就是外层for循环遍历物品，内层for循环遍历背包；
如果求排列数就是外层for循环遍历背包，内层for循环遍历物品。**

**5、举例推导dp数组**

![图示](https://img-blog.csdnimg.cn/20210129144552186.png)
```java
class Solution {
    public int change(int amount, int[] coins) {
        int[] dp = new int[amount + 1];//凑成总金额为 j 的最大组合数是dp[j]
        dp[0] = 1;
        for(int i = 0; i < coins.length; i++){//遍历物品
            for(int j = 1; j <= amount; j++){//遍历背包
                if(j >= coins[i]){
                    dp[j] += dp[j - coins[i]];
                }
            }
        }
        return dp[amount];
    }
}
```

另：

dp[i][j]：**若只使用coins中的前i个硬币的面值，若想凑出金额j，有dp[i][j]种凑法。**

初始化为dp[0][..] = 0， dp[..][0] = 1。因为如果不使用任何硬币面值，就无法凑出任何金额；如果凑出的目标金额为 0，那么“无为而治”就是唯一的一种凑法。

我们最终想得到的答案就是dp[coins.length][amount]。

**如果你不把这第i个物品装入背包**，也就是说你不使用coins[i]这个面值的硬币，那么凑出面额j的方法数dp[i][j]应该等于dp[i-1][j]，继承之前的结果。

**如果你把这第i个物品装入了背包**，也就是说你使用coins[i]这个面值的硬币，那么dp[i][j]应该等于dp[i][j-coins[i-1]]。

首先由于i是从 1 开始的，所以coins的索引是i-1时表示第i个硬币的面值。

dp[i][j-coins[i-1]]也不难理解，如果你决定使用这个面值的硬币，那么就应该关注如何凑出金额j - coins[i-1]。

比如说，你想用面值为 2 的硬币凑出金额 5，那么如果你知道了凑出金额 3 的方法，再加上一枚面额为 2 的硬币，不就可以凑出 5 了嘛。

**综上就是两种选择，而我们想求的dp[i][j]是「共有多少种凑法」，所以dp[i][j]的值应该是以上两种选择的结果之和**
用 Java 写的代码，把上面的思路完全翻译了一遍，并且处理了一些边界问题：
```java
//以下两种都可以
/*求组合时，二维dp数组无论先遍历物品，再遍历背包还是先遍历背包再遍历物品，都是一样的，求出来的都是组合（以下两种求的都是组合），
求不出排列。求排列问题必须用：一维dp先遍历背包再遍历物品。*/
class Solution {
    public int change(int amount, int[] coins) {
        int[][] dp = new int[coins.length + 1][amount + 1];//若只使用coins中的前 i 个硬币的面值，想凑出金额 j，有dp[i][j]种凑法
        for(int i = 0; i <= coins.length; i++){
            dp[i][0] = 1;
        }
        for(int i = 1; i <= coins.length; i++){
            for(int j = 1; j <= amount; j++){
                if(j < coins[i - 1]){
                    dp[i][j] = dp[i - 1][j];
                }else{
                    dp[i][j] = dp[i - 1][j] + dp[i][j - coins[i - 1]];
                }
            }
        }
        return dp[coins.length][amount];
    }
}
class Solution {
    public int change(int amount, int[] coins) {
        int[][] dp = new int[coins.length + 1][amount + 1];//若只使用coins中的前 i 个硬币的面值，想凑出金额 j，有dp[i][j]种凑法
        for(int i = 0; i <= coins.length; i++){
            dp[i][0] = 1;
        }
        for(int j = 1; j <= amount; j++){
            for(int i = 1; i <= coins.length; i++){
                if(j < coins[i - 1]){
                    dp[i][j] = dp[i - 1][j];
                }else{
                    dp[i][j] = dp[i - 1][j] + dp[i][j - coins[i - 1]];
                }
            }
        }
        return dp[coins.length][amount];
    }
}
```