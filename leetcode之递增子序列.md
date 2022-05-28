

491. 递增子序列
给定一个整型数组, 你的任务是找到所有该数组的递增子序列，递增子序列的长度至少是2。
示例:
输入: [4, 6, 7, 7]
输出: [[4, 6], [4, 7], [4, 6, 7], [4, 6, 7, 7], [6, 7], [6, 7, 7], [7,7], [4,7,7]]
说明:
给定数组的长度不会超过15。
数组中的整数范围是 [-100,100]。
给定数组中可能包含重复数字，相等的数字应该被视为递增的一种情况。

思路：
本题求递增子序列，是不能对原数组进行排序的，因为排完序的数组就都是自增子序列了。所以不能之前的去重逻辑了！(之前是通过排序，再加一个标记数组来达到去重的目的)
本题实例还是一个有序数组[4, 6, 7, 7]，更容易误导按照排序的思路去做。
因此下图采用[4, 7, 6, 7]来进行举例
![图示](https://img-blog.csdnimg.cn/20201207101938360.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> findSubsequences(int[] nums) {
        if(nums.length == 0){
            return new ArrayList<>();
        }
        backtracking(nums, 0);
        return res;
    }
    public void backtracking(int[] nums, int startIndex){
        if(list.size() >= 2){
            res.add(new ArrayList<>(list));
            //注意这里不要加return，因为要取树上的所有结点
        }
        //可以没有
        if(startIndex == nums.length){
            return;
        }
        //只要同层重复使用元素，递增子序列就会重复，需要去除掉这种情况
        //还有一种情况就是如果选取的元素小于子序列的最后一个元素，那么就不是递增的，也要去除掉
        HashSet<Integer> set = new HashSet<>();//使用set来对本层元素进行去重
        for(int i = startIndex; i < nums.length; i++){
            //set.contains(nums[i]) 判断nums[i]在本层是否使用过
            if((!list.isEmpty() && nums[i] < list.get(list.size() - 1)) || set.contains(nums[i])){
                continue;
            }
            set.add(nums[i]);//记录该元素在本层用过了，本层后面不能再用了
            list.add(nums[i]);
            backtracking(nums, i + 1);
            //不需要回溯。因为set是记录本层元素是否重复使用，新的一层set会重新定义
            list.remove(list.size() - 1);
        }
    }
}
```
优化：
可以用数组来做哈希，效率会高很多。
注意题目中说，数组范围[-100, 100]，所以可以用数组来做哈希
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> findSubsequences(int[] nums) {
        if(nums.length == 0){
            return new ArrayList<>();
        }
        backtracking(nums, 0);
        return res;
    }
    public void backtracking(int[] nums, int startIndex){
        if(list.size() >= 2){
            res.add(new ArrayList<>(list));
            //注意这里不要加return，因为要取树上的所有结点
        }
        //可以没有
        if(startIndex == nums.length){
            return;
        }
        //只要同层重复使用元素，递增子序列就会重复，需要去除掉这种情况
        //还有一种情况就是如果选取的元素小于子序列的最后一个元素，那么就不是递增的，也要去除掉
        int[] used = new int[201];//使用数组来进行去重操作，因为题目说数组范围[-100, 100]
        for(int i = startIndex; i < nums.length; i++){
            //used[nums[i] + 100] == 1 判断nums[i]在本层是否使用过
            if((!list.isEmpty() && nums[i] < list.get(list.size() - 1)) || used[nums[i] + 100] == 1){
                continue;
            }
            used[nums[i] + 100] = 1;//记录该元素在本层用过了，本层后面不能再用了
            list.add(nums[i]);
            backtracking(nums, i + 1);
            //不需要回溯。因为set是记录本层元素是否重复使用，新的一层set会重新定义
            list.remove(list.size() - 1);
        }
    }
}
```
注：递增子序列 的去重是同一个父结点下的本层的去重。(因此 set 的定义是放在单层搜索里面，而不是放在全局变量)

求子集问题(二) 的去重也是同一父结点下的本层的去重，但是必须要先排序。由下图可知：不排序则会出现重复。

按照下面的示例，因为本题中有一个限制条件，就是子序列必须是递增的，因此在{2, 1} 和 {1, 2}中，只有{1, 2}符合条件，因此不排序也不会出现重复。

![图示哇](https://img-blog.csdnimg.cn/20201209103411708.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)