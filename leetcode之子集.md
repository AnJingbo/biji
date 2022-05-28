

78. 子集
给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。
说明：解集不能包含重复的子集。
示例:
输入: nums = [1,2,3]
输出:
[ [3], [1], [2], [1,2,3], [1,3], [2,3], [1,2], [] ]

思路：
如果把子集问题、组合问题、切割问题都抽象为一棵树的话，那么组合问题和切割问题都是收集树的叶子结点，而子集问题是找树的所有结点![图示](https://img-blog.csdnimg.cn/20201203160831746.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
遍历这个树的时候，把所有结点都记录下来，就是要求的子集集合
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> subsets(int[] nums) {
        if(nums.length == 0){
            return new ArrayList<>();
        }
        backtracking(nums, 0);
        return res;
    }
    //递归函数参数
    public void backtracking(int[] nums, int startIndex){
        res.add(new ArrayList<>(list));//收集子集
        //递归终止条件
        //剩余集合为空的时候，就是叶子结点
        //什么时候剩余集合为空？当startIndex == nums.length的时候。因为没有元素可取了
        //其实也可以不加终止条件，因为startIndex == nums.length，本层for循环就不会进去
        if(startIndex == nums.length){
            return;
        }
        //单层搜索逻辑
        for(int i = startIndex; i < nums.length; i++){
            list.add(nums[i]);
            backtracking(nums, i + 1);
            list.remove(list.size() - 1);
        }
    }
}
```