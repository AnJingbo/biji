

![图示](https://img-blog.csdnimg.cn/26cd6e1cbea64feba9c5ccbd78a319de.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        // 假设数组的元素总和为 sum，添加 + 的元素总和为 x，那么添加 - 的元素总和就是 sum-x。因此 x-(sum-x)=target, 即 x=(target+sum)/2
        int sum = 0;
        for(int num : nums){
            sum += num;
        }
        if(target > sum) return 0;
        if((target + sum) % 2 == 1) return 0;
        int bagSize = (target + sum) / 2;
        if(bagSize < 0) return 0;
        int[][] dp = new int[nums.length + 1][bagSize + 1]; // 从前 i 个物品里任意取，能够装满容量为 j 的方法有 dp[i][j] 种
        dp[0][0] = 1; // 当没有任何元素可以选取时，元素和只能是 0，对应的方案数是 1，
        for(int i = 1; i <= nums.length; i++){
            for(int j = 0; j <= bagSize; j++){
                if(nums[i - 1] > j){
                    dp[i][j] = dp[i - 1][j];
                }else{
                    dp[i][j] = dp[i - 1][j] + dp[i - 1][j - nums[i - 1]];
                }
            }
        }
        return dp[nums.length][bagSize];
    }
}
```