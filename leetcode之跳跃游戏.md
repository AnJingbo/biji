

55. 跳跃游戏
给定一个非负整数数组，你最初位于数组的第一个位置。
数组中的每个元素代表你在该位置可以跳跃的最大长度。
判断你是否能够到达最后一个位置。
示例 1:
输入: [2,3,1,1,4]
输出: true
解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。
示例 2:
输入: [3,2,1,0,4]
输出: false
解释: 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达最后一个位置。

思路：
本题关键不是跳了几步，而是可以跳跃的覆盖范围。这个范围内是一定可以跳到的。
局部最优：每次取最大跳跃步数(取最大覆盖范围)
整体最优：最后得到整体最大覆盖范围，看是否能到终点
局部最优推出全局最优，找不出反例，可以试试贪心。
i 每次移动只能在cover的范围内移动，每移动一个元素，cover得到该元素数值(新的覆盖范围)的补充，让 i 继续移动下去
而cover每次只取 max(该元素数值补充后的范围cover本身范围)
如果cover大于等于了终点下标，直接返回 true

每走一步，就更新最大覆盖范围，而且 i 只能在最大覆盖范围内移动。如果 能覆盖的范围 >= nums.length - 1，说明可以到达终点，否则就不行

覆盖范围 也可以理解为 最远可以到达的位置
```java
class Solution {
    public boolean canJump(int[] nums) {
        if(nums.length == 0 || nums.length == 1){
            return true;
        }
        int cover = 0;
        for(int i = 0; i <= cover; i++){
            cover = Math.max(i + nums[i], cover);
            if(cover >= nums.length - 1){//说明可以覆盖到终点了
                return true;
            }
        }
        /*另一种写法
        for(int i = 0; i < nums.length; i++){
            if(i <= cover){
                cover = Math.max(i + nums[i], cover);
                if(cover >= nums.length - 1){
                    return true;
                }
            }
        }*/
        return false;
    }
}
```