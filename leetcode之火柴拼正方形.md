

473. 火柴拼正方形
还记得童话《卖火柴的小女孩》吗？现在，你知道小女孩有多少根火柴，请找出一种能使用所有火柴拼成一个正方形的方法。不能折断火柴，可以把火柴连接起来，并且每根火柴都要用到。
输入为小女孩拥有火柴的数目，每根火柴用其长度表示。输出即为是否能用所有的火柴拼成正方形。
示例 1:
输入: [1,1,2,2,2]
输出: true
解释: 能拼成一个边长为2的正方形，每边两根火柴。
示例 2:
输入: [3,3,3,3,4]
输出: false
解释: 不能用所有火柴拼成一个正方形。
注意:
给定的火柴长度和在 0 到 10^9之间。
火柴数组的长度不超过15。

本题和 leetcode 698 划分为k个相等的子集 基本一致
```java
class Solution {
    public boolean makesquare(int[] nums) {
        if(nums.length <= 3){
            return false;
        }
        int sum = 0;
        int maxNum = 0;
        for(int i = 0; i < nums.length; i++){
            sum += nums[i];
            maxNum = Math.max(maxNum, nums[i]);
        }
        if(sum % 4 != 0 || maxNum > sum / 4){
            return false;
        }
        boolean[] used = new boolean[nums.length];
        return backtracking(nums, used, 0, 4, 0, sum / 4);
    }
    public boolean backtracking(int[] nums, boolean[] used, int startIndex, int k, int sum, int target){
        if(k == 0){
            return true;
        }
        if(sum > target){
            return false;
        }
        if(sum == target){
            return backtracking(nums, used, 0, k - 1, 0, target);
        }
        for(int i = startIndex; i < nums.length; i++){
            if(used[i] == true){
                continue;
            }
            sum += nums[i];
            used[i] = true;
            if(backtracking(nums, used, i + 1, k, sum, target)){
                return true;
            }
            sum -= nums[i];
            used[i] = false;
        }
        return false;
    }
}
```
代码精简一下：
```java
class Solution {
    public boolean makesquare(int[] nums) {
         if(nums.length <= 3){
            return false;
        }
        int sum = 0;
        int maxNum = 0;
        for(int i = 0; i < nums.length; i++){
            sum += nums[i];
            maxNum = Math.max(maxNum, nums[i]);
        }
        if(sum % 4 != 0 || maxNum > sum / 4){
            return false;
        }
        boolean[] used = new boolean[nums.length];
        return backtracking(nums, used, 0, 4, 0, sum / 4);
    }
    public boolean backtracking(int[] nums, boolean[] used, int startIndex, int k, int sum, int target){
        //回溯算法 遍历整个决策树 寻找将nums拆分成4个子数组是否有可能
        //base case 当k为0时说明可以拼成4个和相同的子数组
        if(k == 0){
            return true;
        }
        //每次选择的数字总和等于target也就是正方形的边长时
        //递归判断剩下的数组是否能拼成k-1个和为target的子数组
        //这里start需要重置为0 但是有used数组在 所以不会重复选择元素
        if(sum == target){
           return backtracking(nums, used, 0, k - 1, 0, target);
        }
        //这里用startIndex来避免大量的无效循环,因为遍历从左到右,在当前节点之前的元素无需再次判断
        for(int i = startIndex; i < nums.length; i++){
            //如果当前元素没有被选择
            //且当前元素加上已经选择的元素总和sum小于target时才会进到决策树的下一节点
            //否则直接跳过
            if(used[i] == false && sum + nums[i] <= target){
                used[i] = true;
                if(backtracking(nums, used, i + 1, k, sum + nums[i], target)){
                    return true;
                }
                used[i] = false;
            }
        }
        return false;
    }
}
```