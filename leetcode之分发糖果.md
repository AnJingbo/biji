

135. 分发糖果
老师想给孩子们分发糖果，有 N 个孩子站成了一条直线，老师会根据每个孩子的表现，预先给他们评分。
你需要按照以下要求，帮助老师给这些孩子分发糖果：
每个孩子至少分配到 1 个糖果。
评分更高的孩子必须比他两侧的邻位孩子获得更多的糖果。
那么这样下来，老师至少需要准备多少颗糖果呢？
示例 1：
输入：[1,0,2]  输出：5
解释：你可以分别给这三个孩子分发 2、1、2 颗糖果。
示例 2：
输入：[1,2,2]  输出：4
解释：你可以分别给这三个孩子分发 1、2、1 颗糖果。
 第三个孩子只得到 1 颗糖果，这已满足上述两个条件。

 思路：
 本题一定是要确定一遍之后再去确定另一边，比如先比较每个孩子的右边然后在比较每一个孩子的左边，如果两边一起考虑一定会顾此失彼
 先确定右边评分大于左边的情况（也就是从前向后遍历）
 此时局部最优：只要右边评分大于左边，右边孩子就多一个糖果
 全局最优：相邻的孩子中，评分高的右孩子比左孩子获得更多的糖果
 如果ratings[i] > ratings[i - 1]，那么 [i] 的糖一定要比 [i - 1] 的糖多一个，所以贪心：candies[i] = candies[i - 1] + 1。
 再确定左边评分大于右边评分的情况（从后向前遍历）
 如果 ratings[i] > ratings[i + 1]，此时candies[i]（第i个小孩的糖果数量）就有两个选择：一个是candies[i + 1] + 1（从右边这个加1得到的糖果数量），一个是candies[i]（之前比较右孩子大于左孩子得到的糖果数量）。
 此时又需要贪心了：
 局部最优：取candies[i + 1] + 1 和 candies[i] 中最大的糖果数量，保证第 i 个小孩的糖果数量既大于左边的也大于右边的。
 全局最优：相邻的孩子中，评分高的孩子获得更多的糖果。
 ![图示](https://img-blog.csdnimg.cn/20201231173033630.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)


```java
class Solution {
    public int candy(int[] ratings) {
        int[] candies = new int[ratings.length];
        Arrays.fill(candies, 1);
        //从前向后
        for(int i = 1; i < ratings.length; i++){
            if(ratings[i] > ratings[i - 1]){
                candies[i] = candies[i - 1] + 1;
            }
        }
        //从后向前
        for(int i = ratings.length - 2; i >= 0; i--){
            if(ratings[i] > ratings[i + 1]){
                candies[i] = Math.max(candies[i], candies[i + 1] + 1);
            }
        }
        //统计结果
        int res = 0;
        for(int num : candies){
            res += num;
        }
        return res;
    }
}
```