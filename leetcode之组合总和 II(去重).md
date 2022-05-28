

40. 组合总和 II
给定一个数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。
candidates 中的每个数字在每个组合中只能使用一次。
说明：
所有数字（包括目标数）都是正整数。
解集不能包含重复的组合。 
示例 :
输入: candidates = [10,1,2,7,6,1,5], target = 8,
所求解集为:
[ [1, 7],[1, 2, 5],[2, 6],[1, 1, 6] ]
思路：
       本题是集合中有重复元素，即组合内可以有重复的元素，但不能有相同的组合。
       将所有组合求出，再去重，容易超时，所以要在搜索中去掉重复组合。
       所谓去重，就是使用过的重复元素不能重复选取。组合问题可以抽象成树形结构，那么"使用过"在树形结构上是有两个维度的，一个维度是在同一树枝上使用过，一个维度是在同一树层使用过。回看题目，本题要去重的是同一树层上的"使用过"，而同一树枝上的都是一个组合里的元素，不用去重
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130220411169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    int sum = 0;
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        boolean[] used = new boolean[candidates.length];
        //去重一定要先对数组进行排序，让其相同的元素都挨在一起，只有这样我们才方便通过相邻的结点来判断是否重复使用了
        Arrays.sort(candidates);
        backtracking(candidates, target, 0, used);
        return res;
    }
    //确定回溯函数的参数和返回值
    //新加了一个used，用来记录同一树枝上的元素是否使用过
    public void backtracking(int[] candidates, int target, int startIndex, boolean[] used){
        //回溯函数的终止条件
        if(sum > target){
            return;
        }
        if(sum == target){
            res.add(new ArrayList<>(list));
            return;
        }
        //单层搜索的逻辑
        //如果candidates[i] == candidates[i - 1] 并且 used[i - 1] == false，
        //就说明：前一个树枝，使用了candidates[i - 1]，也就是说同一树层使用过candidates[i - 1]
        //此时for循环里就应该做continue的操作
        for(int i = startIndex; i < candidates.length; i++){
            // used[i - 1] == true，说明同一树枝candidates[i - 1]使用过
            // used[i - 1] == false，说明同一树层candidates[i - 1]使用过
            // 要对同一树层使用过的元素进行跳过
            //必须要同时有candidates[i] == candidates[i - 1] 和 used[i - 1] == false)
            //比如 1，1；目标值为2，如果没有used[i - 1] == false，那么在到同一个树枝上的第二个1的时候直接就continue了
            if(i > 0 && candidates[i] == candidates[i - 1] && used[i - 1] == false){
                continue;
            }
            sum += candidates[i];
            list.add(candidates[i]);
            used[i] = true;
            backtracking(candidates, target, i + 1, used);
            sum -= candidates[i];
            list.remove(list.size() - 1);
            used[i] = false;
        }
    }
}
```
使用set去重的版本：
注意：使用set去重也必须得先排序。比如: 1、2、1、1，目标值为 3。如果不排序的话会出现重复的解集
注意：使用set去重的版本相对于使用used数组的版本效率要低很多
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    int sum = 0;
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        boolean[] used = new boolean[candidates.length];
        Arrays.sort(candidates);//去重必须要先排序
        backtracking(candidates, target, 0);
        return res;
    }
    public void backtracking(int[] candidates, int target, int startIndex){
        if(sum > target){
            return;
        }
        if(sum == target){
            res.add(new ArrayList<>(list));
            return;
        }
        HashSet<Integer> set = new HashSet<>();
        for(int i = startIndex; i < candidates.length; i++){
            if(set.contains(candidates[i])){
                continue;
            }
            set.add(candidates[i]);
            sum += candidates[i];
            list.add(candidates[i]);
            backtracking(candidates, target, i + 1);
            sum -= candidates[i];
            list.remove(list.size() - 1);
        }
    }
}
```