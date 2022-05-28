

1049. 最后一块石头的重量 II
有一堆石头，每块石头的重量都是正整数。
每一回合，从中选出任意两块石头，然后将它们一起粉碎。假设石头的重量分别为 x 和 y，且 x <= y。那么粉碎的可能结果如下：
如果 x == y，那么两块石头都会被完全粉碎；
如果 x != y，那么重量为 x 的石头将会完全粉碎，而重量为 y 的石头新重量为 y-x。
最后，最多只会剩下一块石头。返回此石头最小的可能重量。如果没有石头剩下，就返回 0。
示例：
输入：[2,7,4,1,8,1]
输出：1
解释：
组合 2 和 4，得到 2，所以数组转化为 [2,7,1,8,1]，
组合 7 和 8，得到 1，所以数组转化为 [2,1,1,1]，
组合 2 和 1，得到 1，所以数组转化为 [1,1,1]，
组合 1 和 1，得到 0，所以数组转化为 [1]，这就是最优值。
提示：
1 <= stones.length <= 30
1 <= stones[i] <= 1000

思路：
本题其实就是尽量让石头分成重量相同的两堆，相撞之后剩下的石头最小，这样就化解成01背包问题了。

本题物品的重量为stones[i]，物品的价值也为stones[i]。
对应着01背包里的物品重量weight[i]和 物品价值value[i]。

动规五步曲：
**1、确定dp数组以及下标的含义**
dp[j]表示容量（这里说容量更形象，其实就是重量）为j的背包，最多可以背dp[j]这么重的石头。

**2、确定递推公式**
01背包的递推公式为：dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
本题：dp[j] = max(dp[j], dp[j - stones[i]] + stones[i]);

**3、dp数组如何初始化**
因为重量都不会是负数，所以dp[j]都初始化为0就可以了，这样在递归公式dp[j] = max(dp[j], dp[j - stones[i]] + stones[i])中dp[j]才不会初始值所覆盖。

**4、确定遍历顺序**
在动态规划：关于01背包问题中已经说明：如果使用一维dp数组，**物品遍历的for循环放在外层，遍历背包的for循环放在内层，且内层for循环倒叙遍历！**

代码如下：
```java
for (int i = 0; i < stones.size(); i++) { // 遍历物品
    for (int j = target; j >= stones[i]; j--) { // 遍历背包
        dp[j] = Math.max(dp[j], dp[j - stones[i]] + stones[i]);
    }
}
```
**5、举例推导dp数组**
举例，输入：[2,4,1,1]，此时target = (2 + 4 + 1 + 1)/2 = 4 ，dp数组状态图如下：
![图示](https://img-blog.csdnimg.cn/20210121194308702.png)
最后dp[target]里是容量为target的背包所能背的最大重量。

那么分成两堆石头，一堆石头的总重量是dp[target]，另一堆就是sum - dp[target]。

在计算target的时候，target = sum / 2 因为是向下取整，所以sum - dp[target] 一定是大于等于dp[target]的。
或者这样理解：背包容量为target，那么由dp[target]的含义可知，dp[target]一定是一个小于等于target的数，因此sum - dp[target] 一定是大于等于dp[target]的。

那么相撞之后剩下的最小石头重量就是 (sum - dp[target]) - dp[target]。
```java
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int sum = 0;
        for(int i = 0; i < stones.length; i++){
            sum += stones[i];
        }
        int target = sum / 2;
        int[] dp = new int[target + 1];
        for(int i = 0; i < stones.length; i++){
            for(int j = target; j >= stones[i]; j--){
                 dp[j] = Math.max(dp[j], dp[j - stones[i]] + stones[i]); 
            }
        }
        return sum - dp[target] - dp[target];
    }
}
```
二维数组方式：
```java
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int sum = 0;
        for(int i = 0; i < stones.length; i++){
            sum += stones[i];
        }
        int target = sum / 2;
        int[][] dp = new int[stones.length + 1][target + 1];//从前 i 个物品里随意取，放入容量为 j 的背包中，最大价值为 dp[i][j]
        for(int i = 1; i <= stones.length; i++){
            for(int j = 1; j <= target; j++){
                if(j < stones[i - 1]){
                    dp[i][j] = dp[i - 1][j];
                }else{
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - stones[i - 1]] + stones[i - 1]);
                }
            }
        }
        return sum - dp[stones.length][target] - dp[stones.length][target];
    }
}
```