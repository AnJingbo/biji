

46. 全排列
给定一个 没有重复 数字的序列，返回其所有可能的全排列。
示例:
输入: [1,2,3]
输出:
[ [1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1] ]
![图示](https://img-blog.csdnimg.cn/20201208094544738.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> permute(int[] nums) {
        if(nums == null){
            return null;
        }
        boolean[] used = new boolean[nums.length];
        backtracking(nums, used);
        return res;
    }
    //递归函数的参数
    //因为排列问题每次都要从头开始搜索，例如 1在[1, 2]中已经使用过，但在[2, 1]中还要使用一次，因此不需要startIndex
    //但需要一个used数组，标记已经选择的元素
    public void backtracking(int[] nums, boolean[] used){
        //递归终止条件
        //可以看出叶子结点，就是收割结果的地方
        //当 list 的大小和 nums 数组一样大的时候，说明找到了一个全排列，也表示到达了叶子结点
        if(list.size() == nums.length){
            res.add(new ArrayList<>(list));
            return;
        }
        //单层搜索的逻辑
        // used 数组其实就是记录此时 list 中都有哪些元素使用过了，一个排列里一个元素只能使用一次
        for(int i = 0; i < nums.length; i++){
            if(used[i] == true){//list 中已经存在的元素，直接跳过
                continue;
            }
            used[i] = true;
            list.add(nums[i]);
            backtracking(nums, used); 
            used[i] = false;   
            list.remove(list.size() - 1);
        }
    }
}
```