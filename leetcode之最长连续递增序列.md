

674. 最长连续递增序列
给定一个未经排序的整数数组，找到最长且 连续递增的子序列，并返回该序列的长度。

连续递增的子序列 可以由两个下标 l 和 r（l < r）确定，如果对于每个 l <= i < r，都有 nums[i] < nums[i + 1] ，那么子序列 [nums[l], nums[l + 1], ..., nums[r - 1], nums[r]] 就是连续递增子序列。

示例 1：
输入：nums = [1,3,5,4,7]
输出：3
解释：最长连续递增序列是 [1,3,5], 长度为3。
尽管 [1,3,5,7] 也是升序的子序列, 但它不是连续的，因为 5 和 7 在原数组里被 4 隔开。 

示例 2：
输入：nums = [2,2,2,2,2]
输出：1
解释：最长连续递增序列是 [2], 长度为1。

提示：
0 <= nums.length <= 104
-109 <= nums[i] <= 109

动规五部曲：

**1、确定dp数组（dp table）以及下标的含义**

dp[i]：以下标i为结尾的数组的连续递增的子序列长度为dp[i]。

注意这里的定义，一定是以下标i为结尾，并不是说一定以下标0为起始位置。

**2、确定递推公式**

如果 nums[i] > nums[i - 1]，那么以 i 为结尾的数组的连续递增的子序列长度 一定等于 以 i-1 为结尾的数组的连续递增的子序列长度 + 1 。

即：dp[i] = dp[i-1] + 1;

**3、dp数组如何初始化**

以下标i为结尾的数组的连续递增的子序列长度最少也应该是1，即就是nums[i]这一个元素。

所以dp[i]应该初始1;

**4、确定遍历顺序**

从递推公式上可以看出， dp[i + 1]依赖dp[i]，所以一定是从前向后遍历。

**5、举例推导dp数组**

已输入nums = [1,3,5,4,7]为例，dp数组状态如下：
![图示](https://img-blog.csdnimg.cn/20210310144350685.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
注意这里要取dp[i]里的最大值，所以dp[2]才是结果！

```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        int len = nums.length;
        if(len == 0 || len == 1){
            return len;
        }
        int[] dp = new int[len];//下标i之前(包括i)的最长递增子序列长度是dp[i]
        Arrays.fill(dp, 1);
        int res = 0;
        for(int i = 1; i < len; i++){
            if(nums[i] > nums[i - 1]){
                dp[i] = dp[i - 1] + 1;
            }
            if(dp[i] > res){
                res = dp[i];
            }
        }
        return res;
    }
}
```