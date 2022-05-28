

300. 最长递增子序列
给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。

子序列是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的子序列。


示例 1：

输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
示例 2：

输入：nums = [0,1,0,3,2,3]
输出：4
示例 3：

输入：nums = [7,7,7,7,7,7,7]
输出：1


提示：

1 <= nums.length <= 2500
-104 <= nums[i] <= 104

思路：

**1、确定dp数组以及下标的含义**

dp[i]表示 以nums[i]这个数为结尾的最长递增子序列的长度

**2、确定状态转移方程**

j从 0 到 i - 1 依次遍历：

**如果 nums[i] > nums[j]，那么 nums[i] 可以跟在 nums[j] 之后（此题要求严格递增），此情况下最长递增子序列的长度为 dp[j] + 1；**

**如果 nums[i] <= nums[j]，那么 nums[i] 无法跟在 nums[j] 之后（此题要求严格递增），此情况下递增子序列不成立，直接跳过。**

所以：

```java
if (nums[i] > nums[j]) dp[i] = Math.max(dp[i], dp[j] + 1);
```

**注意这里不是要dp[i] 与 dp[j] + 1进行比较，而是我们要取dp[j] + 1的最大值。**

**3、dp数组初始化**

每一个i，对应的dp[i]（即最长递增子序列）的起始大小至少都是1。含义是每个元素都至少可以单独成为子序列，此时长度都为 1。


**4、确定遍历顺序**

dp[i]是由 0 到 i-1各个位置的最长递增子序列推导而来，那么遍历 i 一定是从前向后遍历

j 其实就是 0 到 i-1，遍历 i 的循环在外层，遍历 j 则在内层。代码如下：

```java
for(int i = 0; i < len; i++){
    for(int j = 0; j < i; j++){
        if(nums[i] > nums[j]){
            dp[i] = Math.max(dp[i], dp[j] + 1);
        }
    }
    if(dp[i] > res){
        res = dp[i];
    }
}
```

**5、打印推导dp数组**

输入：[0, 1, 0, 3, 2]，dp数组的变化如下：
![图示](https://img-blog.csdnimg.cn/2021030919441244.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int len = nums.length;
        int[] dp = new int[len];//以nums[i]这个数为结尾的最长递增子序列的长度
        Arrays.fill(dp, 1);
        int res = 0;
        for(int i = 0; i < len; i++){
            for(int j = 0; j < i; j++){
                if(nums[i] > nums[j]){
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            if(dp[i] > res){
                res = dp[i];
            }
        }
        return res;
    }
}
```