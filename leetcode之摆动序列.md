

376. 摆动序列
如果连续数字之间的差严格地在正数和负数之间交替，则数字序列称为摆动序列。第一个差（如果存在的话）可能是正数或负数。少于两个元素的序列也是摆动序列。
例如， [1,7,4,9,2,5] 是一个摆动序列，因为差值 (6,-3,5,-7,3) 是正负交替出现的。相反, [1,4,7,2,5] 和 [1,7,4,5,5] 不是摆动序列，第一个序列是因为它的前两个差值都是正数，第二个序列是因为它的最后一个差值为零。
给定一个整数序列，返回作为摆动序列的最长子序列的长度。 通过从原始序列中删除一些（也可以不删除）元素来获得子序列，剩下的元素保持其原始顺序。
示例 :
输入: [1,17,5,10,13,15,10,5,16,8]
输出: 7
解释: 这个序列包含几个长度为 7 摆动序列，其中一个可为[1,17,10,13,10,16,8]。
思路：![图示](https://img-blog.csdnimg.cn/20201215163314784.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
局部最优：删除单调坡度上的结点(不包括单调坡度两端的结点)，那么这个坡度就可以有两个局部峰值
整体最优：整个序列有最多的局部峰值，从而达到最长摆动序列
局部最优推出全部最优，并且举不出反例，那就试试贪心。
实际操作上，其实连删除的操作都不用。因为题目要求的是最长摆动子序列的长度，所以只需要统计数组的峰值数量就可以了(相当于是删除单一坡度上的结点，然后统计长度)
这就是贪心所贪的地方：让峰值尽可能的保持峰值，然后删除单一坡度上的结点
本题技巧：比如统计峰值的时候，数组最左面和最右面是最不好统计的。
例如序列[2, 5]，它的峰值数量是 2，如果靠统计差值来计算峰值个数就需要考虑数组最左面和最右面的特殊情况。
所以针对[2, 5]，可以假设为[2, 2, 5]，这样它就有了坡度，即preDiff = 0，如图：
![图示1](https://img-blog.csdnimg.cn/20201215164038819.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
针对以上情形，res 初始为1(默认最右面有一个峰值)，此时curDiff > 0 && preDiff <= 0，那么 res++(计算了左面的峰值)，最后得到的 res 就是2(峰值个数 2 即为摆动序列长度为 2)

```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if(nums.length <= 1){
            return nums.length;
        }
        int curDiff = 0;//当前一对差值
        int preDiff = 0;//前一对差值
        int res = 1;//记录峰值个数，序列默认最右边有一个峰值
        for(int i = 1; i < nums.length; i++){
            curDiff = nums[i] - nums[i - 1];
            //出现峰值
            if((curDiff > 0 && preDiff <= 0) || (curDiff < 0 && preDiff >= 0 )){
                res++;
                preDiff = curDiff;//注意：不能写在if语句外面。例如：2, 1, 1, 0
            }
        }
        return res;
    }
}
```
自己写的：
```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if(nums == null || nums.length == 0){
            return 0;
        }
        if(nums.length == 1){
            return 1;
        }
        int res = 0;
        int res1 = 0;
        int[] temp = new int[nums.length - 1];
        int index = 0;
        for(int i = 0; i < nums.length; i++){
            if(i + 1 < nums.length){
                temp[index++] = nums[i + 1] - nums[i]; 
            }
        }
        int place = 0;
        int place1 = 0;
        for(int i = 0; i < temp.length; i++){
            //以下是先正后负的情况
            if(place % 2 == 0 && temp[i] > 0){
                res++;
                place++;
            }else if(place % 2 == 1 && temp[i] < 0){
                res++;
                place++;
            }
            //以下是先负后正的情况
            if(place1 % 2 == 0 && temp[i] < 0){
                res1++;
                place1++;
            }else if(place1 % 2 == 1 && temp[i] > 0){
                res1++;
                place1++;
            }
            
        }
        return res > res1 ? res + 1 : res1 + 1;
    }
}
```