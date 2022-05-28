

198. 打家劫舍
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

示例 1：
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
示例 2：
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
偷窃到的最高金额 = 2 + 9 + 1 = 12 。

提示：
0 <= nums.length <= 100
0 <= nums[i] <= 400

思路：

回溯（超时）：
```java
class Solution {
    int res = Integer.MIN_VALUE;
    int temp = 0;
    public int rob(int[] nums) {
        backtracking(nums, 0);
        return res;
    }
    public void backtracking(int[] nums, int startIndex){
        if(startIndex >= nums.length){
            res = Math.max(res, temp);
            return;
        }
        for(int i = startIndex; i < nums.length; i++){
            temp += nums[i];
            backtracking(nums, i + 2);
            temp -= nums[i];
        }
    }
}
```

动规五部曲：

**1、确定dp数组以及下标的含义**

dp[i]：从下标为i(包括i)的房子之前任意取，能够得到的最高金额是dp[i]。

**2、确定递推公式**

决定dp[i]的因素是第 i 间房子偷还是不偷。

如果偷第 i 间房子，那么第 i - 1 间房子一定是不考虑的，所以要找出 从前 i - 1 个房子里任意取，能够得到的最大价值dp[i - 2] 加上 第i间房子可以偷到的钱，即dp[i] = dp[i - 2] + nums[i];

如果不偷第 i 间房子，那么只考虑前 i-1 间房就行，即dp[i] = dp[i - 1];

然后dp[i]取最大值，即dp[i] = Math.max(dp[i - 2] + nums[i], dp[i - 1]);

**3、如何初始化**

由递推公式Math.max(dp[i - 2] + nums[i], dp[i - 1])可得：递推公式的基础就是dp[0]和dp[1]。

从dp[i]的定义上来说，dp[0]一定是nums[0]，dp[1]就是nums[0]和nums[1]的最大值，即dp[2] = Math.max(nums[0], nums[1]);

**4、确定遍历顺序**

dp[i]是根据dp[i - 2]和dp[i - 1]推导出来的，那么一定是从前向后遍历。

**5、打印dp数组**
输入：[2, 7, 9, 3, 1]
![图示](https://img-blog.csdnimg.cn/20210222144735557.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
class Solution {
    public int rob(int[] nums) {
        if(nums.length == 0){
            return 0;
        }
        if(nums.length == 1){
            return nums[0];
        }
        int[] dp = new int[nums.length];//从下标为i(包括i)的房子之前任意取，能够得到的最高金额是dp[i]
        
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);

        for(int i = 2; i < nums.length; i++){
            dp[i] = Math.max(dp[i - 2] + nums[i], dp[i - 1]);
        }
        return dp[nums.length - 1];
    }
    
}
```