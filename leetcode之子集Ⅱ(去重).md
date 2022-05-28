

90. 子集 II
给定一个可能包含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。
说明：解集不能包含重复的子集。
示例:
输入: [1,2,2]
输出:
[ [2],[1], [1,2,2],[2,2],[1,2],[] ]

由下图可知：其实在不同树层上都会判断是否重复读取
![图示](https://img-blog.csdnimg.cn/20201206152523713.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        if(nums.length == 0){
            return new ArrayList<>();
        }
        boolean[] used = new boolean[nums.length];
        //注意：去重一定要对集合进行排序，只有这样我们才方便通过相邻的结点来判断是否重复使用了
        Arrays.sort(nums);
        backtracking(nums, 0, used);
        return res;
    }
    public void backtracking(int[] nums, int startIndex, boolean[] used){
        res.add(new ArrayList<>(list));
        if(startIndex == nums.length){
            return;
        }
        //如果nums[i] == nums[i - 1] 并且 used[i - 1] == false，
        //就说明：出现了相同元素并且在同一树层上已经使用过了，同时保证了在同一树枝上时不会直接continue
        //此时for循环里就应该做continue的操作
        for(int i = startIndex; i < nums.length; i++){
           // used[i - 1] == true，说明同一树枝nums[i - 1]使用过
           // used[i - 1] == false，说明同一树层nums[i - 1]使用过
           //如果只有used[i  - 1] == false，那么只能说明在同一树层上已经使用过，但不能说明是重复数字
           //如果只有nums[i] == nums[i - 1]，那么同一树枝、同一树层上的重复数字都会去除掉
            if(i > 0 && nums[i] == nums[i - 1] && used[i  - 1] == false){
                continue;
            }
            used[i] = true;
            list.add(nums[i]);
            backtracking(nums, i + 1, used);
            used[i] = false;
            list.remove(list.size() - 1);
        }
    }
}
```
使用set去重的版本：
注意：使用set去重也必须得先排序。比如: 1、2、1、1。如果不排序的话会出现重复的子集
使用set去重的版本相对于使用used数组的版本效率要低很多
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        if(nums.length == 0){
            return new ArrayList<>();
        }
        Arrays.sort(nums);//去重必须要先排序
        backtracking(nums, 0);
        return res;
    }
    public void backtracking(int[] nums, int startIndex){
        res.add(new ArrayList<>(list));
        if(startIndex == nums.length){
            return;
        }
        HashSet<Integer> set = new HashSet<>();
        for(int i = startIndex; i < nums.length; i++){
            if(set.contains(nums[i])){
                continue;
            }
            set.add(nums[i]);
            list.add(nums[i]);
            backtracking(nums, i + 1);
            list.remove(list.size() - 1);
        }
    }
}
```