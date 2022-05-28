

39. 组合总和
给定一个无重复元素的数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。
candidates 中的数字可以无限制重复被选取。
说明：
所有数字（包括 target）都是正整数。
解集不能包含重复的组合。 
示例 ：
输入：candidates = [2,3,6,7], target = 7,
所求解集为：
[
    [7],
    [2,2,3]
]

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    int sum = 0;
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
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
        for(int i = startIndex; i < candidates.length; i++){
            sum += candidates[i];
            list.add(candidates[i]);
            backtracking(candidates, target, i);
            sum -= candidates[i];
            list.remove(list.size() - 1);
        }
    }
}
```
剪枝：
对于sum已经大于target的情况，其实依然进入了下一层递归，只是下一层递归结束判断的时候，会判断sum > target的话就返回。
其实如果已经知道下一层的sum会大于target，就没有必要进入下一层递归了
对总集合排序之后，如果下一层的sum(就是本层的sum + candidates[i])已经大于target，就可以结束本轮for循环了(只有排序了，才能知道后面的一定不可以了，都是越来越大的数)

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    int sum = 0;
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);
        backtracking(candidates, target, 0);
        return res;
    }
    public void backtracking(int[] candidates, int target, int startIndex){
        if(sum == target){
            res.add(new ArrayList<>(list));
            return;
        }
        //如果sum + candidates[i] > target就终止遍历
        for(int i = startIndex; i < candidates.length && sum + candidates[i] <= target; i++){
            sum += candidates[i];
            list.add(candidates[i]);
            backtracking(candidates, target, i);
            sum -= candidates[i];
            list.remove(list.size() - 1);
        }
    }
}
```