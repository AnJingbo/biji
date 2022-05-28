

416. 分割等和子集
给定一个只包含正整数的非空数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。
注意:
每个数组中的元素不会超过 100
数组的大小不会超过 200
示例 1:
输入: [1, 5, 11, 5]
输出: true
解释: 数组可以分割成 [1, 5, 5] 和 [11].
示例 2:
输入: [1, 2, 3, 5]
输出: false
解释: 数组不能分割成两个元素和相等的子集.

思路:
暴力回溯（会超时）：
```java
class Solution {
    int temp = 0;
    boolean res = false;
    public boolean canPartition(int[] nums) {
        if(nums.length <= 1){
            return false;
        }
        int sum = 0;
        for(int i = 0; i < nums.length; i++){
            sum += nums[i];
        }
        backtracking(nums, 0, sum);
        return res;
    }
    public void backtracking(int[] nums, int startIndex, int sum){   
        if(temp == sum - temp){
            res = true;
            return;
        }
        for(int i = startIndex; i < nums.length; i++){
            temp += nums[i];
            backtracking(nums, i + 1, sum);
            temp -= nums[i];
        }
    } 
}
```

因为本题中每个元素只能使用一次，所以本题是01背包问题。
首先，本题要求集合里是否出现总和为sum / 2的子集。
只有确定了如下四点，才能把01背包问题套入本题上：
(1) 背包的体积是sum / 2；
(2) 背包要放入的物品重量为集合中元素的数值，价值也为集合中元素的数值；
(3) 背包如果正好装满，说明找到了总和为sum / 2的子集；
(4) 背包中每一个元素是不可重复放入的(所以是01背包问题)。

动规五部曲：

**1、确定dp数组以及下标的含义**
01背包中，dp[j] 表示：容量为j的背包，所背的物品价值可以最大为dp[j]。

套到本题，dp[j]表示 背包总容量是j，最大可以凑成i的子集总和为dp[j]。

**2、确定递推公式**
01背包的递推公式为：dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);

本题，相当于背包里放入数值，那么物品i的重量是nums[i]，其价值也是nums[i]。

所以递推公式：dp[j] = max(dp[j], dp[j - nums[i]] + nums[i]);

**3、dp数组如何初始化**
在01背包，一维dp如何初始化，已经讲过，

从dp[j]的定义来看，首先dp[0]一定是0。

如果题目给的价值都是正整数那么非0下标都初始化为0就可以了，如果题目给的价值有负数，那么非0下标就要初始化为负无穷。

这样才能让dp数组在递归公式的过程中取的最大的价值，而不是被初始值覆盖了。

本题题目中 只包含正整数的非空数组，所以非0下标的元素初始化为0就可以了。

**4、确定遍历顺序**
在动态规划：关于01背包问题中就已经说明：**如果使用一维dp数组，物品遍历的for循环放在外层，遍历背包的for循环放在内层，且内层for循环倒叙遍历！**

代码如下：

```java
// 开始 01背包 
for(int i = 0; i < nums.length; i++) {
    for(int j = target; j >= nums[i]; j--) { // 每一个元素一定是不可重复放入，所以从大到小遍历
        dp[j] = Math.max(dp[j], dp[j - nums[i]] + nums[i]);
    }
}
```

**5、举例推导dp数组**
dp[j]的数值一定是小于等于 j 的，因为dp[j]表示 背包总容量是j，最大可以凑成i的子集总和为dp[j]。

**如果dp[j] == j 说明，集合中的子集总和正好可以凑成总和j**，理解这一点很重要。

用例1，输入[1,5,11,5] 为例，如图：
![图示](https://img-blog.csdnimg.cn/20210121200329456.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
最后dp[11] == 11，说明可以将这个数组分割成两个子集，使得两个子集的元素和相等。
```java
class Solution {
    public boolean canPartition(int[] nums) {
        if(nums.length < 2){
            return false;
        }
        int sum = 0;
        for(int i = 0; i < nums.length; i++){
            sum += nums[i];
        }
        if(sum % 2 == 1){
            return false;
        }
        int target = sum / 2;
        int[][] dp = new int[nums.length + 1][target + 1];//从前 i 个物品里面任意取，使得容量为 j 的背包的最大重量是dp[i][j]
        for(int i = 1; i <= nums.length; i++){
            for(int j = 1; j <= target; j++){
                if(j < nums[i - 1]){
                    dp[i][j] = dp[i - 1][j];
                }else{
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - nums[i - 1]] + nums[i - 1]);
                }
            }
        }
        return dp[nums.length][target] == target;
    }
}
```
滚动数组：
```java
class Solution {
    public boolean canPartition(int[] nums) {
        if(nums.length <= 1){
            return false;
        }
        int sum = 0;
        for(int i = 0; i < nums.length; i++){
            sum += nums[i];
        }
        if(sum % 2 == 1){
            return false;
        }
        int target = sum / 2;
        int[] dp = new int[target + 1];
        for(int i = 0; i < nums.length; i++){
            for(int j = target; j >= nums[i]; j--){//每一个元素一定是不可重复放入，所以从大到小遍历
                dp[j] = Math.max(dp[j], dp[j - nums[i]] + nums[i]);
            }
        }
        return dp[target] == target;
    }
}
```