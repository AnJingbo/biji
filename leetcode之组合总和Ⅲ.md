

216. 组合总和 III
找出所有相加之和为 n 的 k 个数的组合。组合中只允许含有 1 - 9 的正整数，并且每种组合中不存在重复的数字。
说明：
1、所有数字都是正整数。
2、解集不能包含重复的组合。 
示例 1:
输入: k = 3, n = 7
输出: [[1,2,4]]
示例 2:
输入: k = 3, n = 9
输出: [[1,2,6], [1,3,5], [2,3,4]]

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> combinationSum3(int k, int n) {
        backtracking(k, n, 0, 0);
        return res;
    }
    public void backtracking(int k, int n, int sum, int startIndex){
        if(list.size() == k && sum == n){
            res.add(new ArrayList(list));
            return;
        }
        for(int i = startIndex; i < 9; i++){
            //注意：处理和回溯是一一对应的
            sum += (i + 1);//处理
            list.add(i + 1);//处理
            backtracking(k, n, sum, i + 1);
            sum -= (i + 1);//回溯
            list.remove(list.size() - 1);//回溯
        }
    }
}
//自己写的
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> combinationSum3(int k, int n) {
        backtracking(k, n, 0, 0);
        return res;
    }
    public void backtracking(int k, int n, int sum, int startIndex){
        if(list.size() == k && sum == n){
            res.add(new ArrayList(list));
            return;
        }
        for(int i = startIndex; i < 9; i++){
            list.add(i + 1);
            backtracking(k, n, sum + (i + 1), i + 1);//隐含着回溯
            list.remove(list.size() - 1);
        }
    }
}

//剪枝：如果元素总和已经大于n，那么往后遍历就没有意义了，直接剪掉
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> combinationSum3(int k, int n) {
        backtracking(k, n, 0, 0);
        return res;
    }
    public void backtracking(int k, int n, int sum, int startIndex){
        if(sum > n){//剪枝操作
            return;
        }
        if(list.size() == k && sum == n){
            res.add(new ArrayList(list));
            return;
        }
        for(int i = startIndex; i < 9; i++){
            //注意：处理和回溯是一一对应的
            sum += (i + 1);//处理
            list.add(i + 1);//处理
            backtracking(k, n, sum, i + 1);
            sum -= (i + 1);//回溯
            list.remove(list.size() - 1);//回溯
        }
    }
}
```