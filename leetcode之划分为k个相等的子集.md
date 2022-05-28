

698. 划分为k个相等的子集
给定一个整数数组  nums 和一个正整数 k，找出是否有可能把这个数组分成 k 个非空子集，其总和都相等。

示例 1：

输入： nums = [4, 3, 2, 3, 5, 2, 1], k = 4
输出： True
说明： 有可能将其分成 4 个子集（5），（1,4），（2,3），（2,3）等于总和。


提示：

1 <= k <= len(nums) <= 16
0 < nums[i] < 10000

本题和 leetcode 473 火柴拼正方形 基本一致
```java
class Solution {
    public boolean canPartitionKSubsets(int[] nums, int k) {
        int sum = 0;
        int maxNum = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            maxNum = Math.max(maxNum, nums[i]);
        }
        if (sum % k != 0 || maxNum > sum / k) {
            return false;
        }
        int target = sum / k;
        boolean[] used = new boolean[nums.length];
        return backtracking(nums, used, 0, k, 0, target);
    }

    public boolean backtracking(int[] nums, boolean[] used, int startIndex, int k, int sum, int target) {
        if (sum > target) {
            return false;
        }
        if (k == 0) {
            return true;
        }
        if (sum == target) {
            return backtracking(nums, used, 0, k - 1, 0, target);
        }
        for (int i = startIndex; i < nums.length; i++) {
            if (used[i] == true) {
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
    public boolean canPartitionKSubsets(int[] nums, int k) {
        int sum = 0;
        int maxNum = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            maxNum = Math.max(maxNum, nums[i]);
        }
        if (sum % k != 0 || maxNum > sum / k) {
            return false;
        }
        int target = sum / k;
        boolean[] used = new boolean[nums.length];
        return backtracking(nums, used, 0, k, 0, target);
    }

     public boolean backtracking(int[] nums, boolean[] used, int startIndex, int k, int sum, int target){
        //回溯算法 遍历整个决策树 寻找将nums拆分成k个子数组是否有可能
        //base case 当k为0时说明可以拼成k个和相同的子数组
        if(k == 0){
            return true;
        }
        //每次选择的数字总和等于target的时候
        //递归判断剩下的数组是否能拼成k-1个和为target的子数组
        //这里start需要重置为0 但是有used数组在 所以不会重复选择元素
        if(sum == target){
           return backtracking(nums, used, 0, k - 1, 0, target);
        }
        //这里用startIndex来避免大量的无效循环,因为遍历从左到右,在当前节点之前的元素无需再次判断
        for(int i = startIndex; i < nums.length; i++){
            //如果当前元素没有被选择
            //且当前元素加上已经选择的元素总和sum小于target时才会进到决策树的下一结点
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