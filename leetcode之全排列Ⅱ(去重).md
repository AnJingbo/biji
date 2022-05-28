

47. 全排列 II
给定一个可包含重复数字的序列 nums ，按任意顺序 返回所有不重复的全排列。
示例 1：
输入：nums = [1,1,2]
输出：
[ [1,1,2], [1,2,1], [2,1,1] ]
示例 2：
输入：nums = [1,2,3]
输出：[ [1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1] ]

![图示](https://img-blog.csdnimg.cn/20201208105643257.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
自己写的：
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> permuteUnique(int[] nums) {
        if(nums == null){
            return null;
        }
        //去重一定要对元素进行排序，只有这样我们才方便通过相邻的结点来判断是否重复使用了
        Arrays.sort(nums);
        boolean[] used = new boolean[nums.length];
        boolean[] judge = new boolean[nums.length];//树层去重，这个就是之前组合去重时的used
        backtracking(nums, used, judge);
        return res;
    }
    public void backtracking(int[] nums, boolean[] used, boolean[] judge){
        if(list.size() == nums.length){
            res.add(new ArrayList<>(list));
            return;
        }
        for(int i = 0; i < nums.length; i++){
            if(i > 0 && nums[i] == nums[i - 1] && judge[i - 1] == false){
                continue;
            }
            if(used[i] == true){
                continue;
            }
            judge[i] = true;
            used[i] = true;
            list.add(nums[i]);
            backtracking(nums, used, judge);
            judge[i] = false;
            used[i] = false;
            list.remove(list.size() - 1);
        }
    }
}
```
借鉴的：
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    public List<List<Integer>> permuteUnique(int[] nums) {
        if(nums == null){
            return null;
        }
        //去重一定要对元素进行排序，只有这样我们才方便通过相邻的结点来判断是否重复使用了
        Arrays.sort(nums);
        boolean[] used = new boolean[nums.length];
        backtracking(nums, used);
        return res;
    }
    public void backtracking(int[] nums, boolean[] used){
        if(list.size() == nums.length){
            res.add(new ArrayList<>(list));
            return;
        }
        for(int i = 0; i < nums.length; i++){
            // used[i - 1] == true，说明同一树枝nums[i - 1]使用过
            // used[i - 1] == false，说明同一树层nums[i - 1]使用过
            //如果同一树层nums[i - 1]使用过则直接跳过
            if(i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false){
                continue;
            }
            if(used[i] == true){// list 中已经存在的元素，直接跳过
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
拓展：
if(i > 0 && nums[i] == nums[i - 1] && used[i - 1] == true) 也是正确的！
如果要对树层中前一位去重，就用 used[i - 1] == false，如果要对树枝前一位进行去重，就用 used[i - 1] == true。
对于排列问题，树层上去重和树枝上去重，都是可以的，但是树层上去重效率更高
比如：[1, 1, 1]
树层上去重(used[i - 1] == false)的树形结构如下：![树层去重](https://img-blog.csdnimg.cn/20201208110816431.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
树枝上去重(used[i - 1] == true)的树形结构如下：
![树枝去重](https://img-blog.csdnimg.cn/20201208110847844.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)