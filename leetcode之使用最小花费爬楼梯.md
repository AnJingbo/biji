

746. 使用最小花费爬楼梯
数组的每个下标作为一个阶梯，第 i 个阶梯对应着一个非负数的体力花费值 cost[i]（下标从 0 开始）。
每当你爬上一个阶梯你都要花费对应的体力值，一旦支付了相应的体力值，你就可以选择向上爬一个阶梯或者爬两个阶梯。
请你找出达到楼层顶部的最低花费。在开始时，你可以选择从下标为 0 或 1 的元素作为初始阶梯。
示例 1：
输入：cost = [10, 15, 20]
输出：15
解释：最低花费是从 cost[1] 开始，然后走两步即可到阶梯顶，一共花费 15 
示例 2：
输入：cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
输出：6
解释：最低花费方式是从 cost[0] 开始，逐个经过那些 1 ，跳过 cost[3] ，一共花费 6 。
提示：
cost 的长度范围是 [2, 1000]。
cost[i] 将会是一个整型数据，范围为 [0, 999] 。

自己写的：
```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        if(cost.length == 2){
            return Math.min(cost[0], cost[1]);
        }
        int res = 0;
        int[] dp = new int[cost.length];//到达第i个阶梯所需要的最小体力
        dp[0] = 0;
        dp[1] = 0;
        for(int i = 2; i < dp.length; i++){
        /*到达第i个阶梯所需要的最小体力 = (到达第i - 1个阶梯所需最小体力 + 从第i - 1个阶梯跳到第i个阶梯所需体力)
        和 (到达第i - 2个阶梯所需最小体力 + 从第i - 2个阶梯跳到第i个阶梯所需体力) 的最小值*/
            dp[i] = Math.min(cost[i - 1] + dp[i - 1], cost[i - 2] + dp[i - 2]);
            if(i + 1 == dp.length){
                res = Math.min(dp[i - 1] + cost[i - 1], dp[i] + cost[i]);
            }
        }
        return res;
    }
}
```
优化一下代码：
```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        if(cost.length == 2){
            return Math.min(cost[0], cost[1]);
        }
        int len = cost.length + 1;
        int[] dp = new int[len];//到达某一个阶梯所需要的最小体力
        dp[0] = 0;
        dp[1] = 0;
        for(int i = 2; i < len; i++){
            /*到达第i个阶梯所需要的最小体力 = (到达第i - 1个阶梯所需最小体力 + 从第i - 1个阶梯跳到第i个阶梯所需体力)和 (到达第i - 2个阶梯所需最小体力 + 从第i - 2个阶梯跳到第i个阶梯所需体力) 的最小值*/
            dp[i] = Math.min(cost[i - 1] + dp[i - 1], cost[i - 2] + dp[i - 2]);
            
        }
        return dp[len - 1];
    }
}
```