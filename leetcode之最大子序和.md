

53. 最大子序和
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
示例:
输入: [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。

思路:
如果 -2, 1 在一起，计算起点的时候，一定是从1开始计算，因为负数只会拉低总和，这就是贪心算法所贪的地方。
局部最优：当前"连续和"为负数的时候立刻放弃，从下一个元素重新计算"连续和"，因为负数加上下一个元素，"连续和"只会越来越小
全局最优：选取最大"连续和"
关键：不能让"连续和"为负数的时候加上下一个元素，而不是不让"连续和"加上一个负数
局部最优的情况下，并记录最大的"连续和"，可以推出全局最优
从代码角度上讲：遍历 nums数组，从头开始用sum累积，如果sum一旦加上nums[i]变成负数，那么就应该从nums[i + 1]开始从0累积sum了，因为已经变成负数的sum，只会拖累总和。
贪心算法：
```java
class Solution {
    public int maxSubArray(int[] nums) {
        if(nums.length == 1){
            return nums[0];
        }
        int res = Integer.MIN_VALUE;
        int sum = 0;
        for(int i = 0; i < nums.length; i++){
            sum += nums[i];
            if(res < sum){//取区间累积的最大值(相当于不断确定最大子序的终止位置)
                res = sum;
            }
            if(sum <= 0){
                sum = 0;//相当于重置最大子序的起始位置，因为遇到负数一定是拉低总和
            }
        }
        return res;
    }
}
```
暴力解法：
```java
class Solution {
    public int maxSubArray(int[] nums) {
        if(nums.length == 1){
            return nums[0];
        }
        int res = Integer.MIN_VALUE;
        for(int i = 0; i < nums.length; i++){
            int sum= 0;
            for(int j = i; j < nums.length; j++){
                sum += nums[j];
                res = res < sum ? sum : res;
            }
        }
        return res;
    }
}
```