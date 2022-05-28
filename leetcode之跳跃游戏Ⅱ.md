

45. 跳跃游戏 II
给定一个非负整数数组，你最初位于数组的第一个位置。
数组中的每个元素代表你在该位置可以跳跃的最大长度。
你的目标是使用最少的跳跃次数到达数组的最后一个位置。
示例:
输入: [2,3,1,1,4]
输出: 2
解释: 从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
说明: **假设你总是可以到达数组的最后一个位置**。
思路：如果某一个作为起跳点的格子的跳跃距离是3，那么表示后面3个格子都可以作为起跳点。可以对每个可以跳跃的格子都起跳一遍，不断更新能跳到的最远距离。每跳完一次后，需要更新下一次起跳点的范围，在新的范围内跳，更新能跳到的最远距离
```java
class Solution {
    public int jump(int[] nums) {
        if(nums.length == 1){
            return 0;
        }
        int start = 0;
        int end = 1;
        int num = 0;
        while(end < nums.length){
            int cover = 0;
            for(int i = start; i < end; i++){
                cover = Math.max(cover, i + nums[i]);//能跳到最远的距离
            }
            start = end;//下一次起跳点范围开始的格子
            end = cover + 1;//下一次起跳点范围结束的格子
            num++;//跳跃次数
        }
        return num;
    }
}
```
优化：
从上面的代码中发现，其实被while包含的for循环中，i是从start跑到end的。其实只需要在一次跳跃完成时，更新下一次能跳到最远的距离，并以此刻为时机更新跳跃次数，就可以在一次for循环中处理

```java
class Solution {
    public int jump(int[] nums) {
        if(nums.length == 1){
            return 0;
        }
        int end = 0;
        int num = 0;
        int cover = 0;
        for(int i = 0; i < nums.length - 1; i++){
            cover = Math.max(cover, i + nums[i]);
            if(i == end){
                end = cover;
                num++;
            }
        }
        return num;
    }
}
```