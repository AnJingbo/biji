

### twoSum 问题
* 给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出能够凑出目标值 target 的两个元素的值（注意：不是下标）。
```java
public int[] twoSum(int[] nums, int target) {
    Arrays.sort(nums);
    int low = 0, high = nums.length - 1;
    while(low < high){
        int sum = nums[low] + nums[high];
        if(sum < target){
            low++;
        }else if(sum > target){
            high--;
        }else{
            return new int[]{nums[low], nums[high]};
        }
    }
    return new int[0];
}
```
### 泛化的 twoSum 问题
* 如果 nums 中有多对元素之和都等于 target，请你的算法**返回所有和为 target 的元素对，并且其中不能出现重复的二元组。**
* 比如输入：nums = { 1, 3, 1, 2, 2, 3 }，那么算法返回的结果就是：[[1, 3], [2, 2]]
* 对于修改后的问题，关键难点是现在可能有多个和为 target 的数对，还不能重复。比如上述例子中 [1, 3] 和 [3, 1] 虽然都符合要求，但是重复了，只能算一次。
* 出问题的地方在于：**当 sum == target 时，在给 res 加入一次结果后，low 和 high 不应该只是改变 1，而是应该跳过所有重复的元素。**
```java
public static List<List<Integer>> twoSumTarget(int[] nums, int target){
    Arrays.sort(nums);
    List<List<Integer>> res = new ArrayList<>();
    int low = 0, high = nums.length - 1;
    while(low < high){
        int sum = nums[low] + nums[high];
        int left = nums[low], right = nums[high];
        if(sum < target){
            while(low < high && nums[low] == left){
                low++;
            }
        }else if(sum > target){
            while(low < high && nums[high] == right){
                high--;
            }
        }else{
            List<Integer> list = new ArrayList<>();
            list.add(nums[low]);
            list.add(nums[high]);
            res.add(new ArrayList<>(list));
            while(low < high && nums[low] == left){
                low++;
            }
            while(low < high && nums[high] == right){
                high--;
            }
        }
    }
    return res;
}
```
* 时间复杂度：排序的时间复杂度是 $O(NlogN)$，双指针操作的部分虽然有很多 while 循环，但是时间复杂度还是 $O(N)$。所以这个函数的时间复杂度是 $O(NlogN)$
### 3Sum 问题
* 给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a, b, c，使得 a + b + c = target ？请你找出所有满足条件且不重复的三元组。
* 比如：
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;输入：nums = [-1,0,1,2,-1,-4], target = 0
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;输出：[ [-1,-1,2], [-1,0,1] ]
* 思路：在确定了三元组中的第一个数字之后，我们需要找的就是剩下的两个和为 target - nums[i] 的数字，而这就是 twoSumTarget 函数所解决的问题。另外，三元组也可能会出现重复，比如当 nums = [1, 1, 1, 2, 3]，target = 6 时，结果就会重复。**关键点在于，不能让第一个数字重复，至于后面的两个数，twoSumTarget 函数会保证他们不重复。** 因此我们需要在代码中用一个 while 循环来保证每个三元组中的第一个元素不重复。
```java
class Solution {
    public List<List<Integer>> threeSumTarget(int[] nums, int target) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        // 枚举三元组中的第一个数
        for(int i = 0; i < nums.length; i++){
            List<List<Integer>> temp = twoSumTarget(nums, target - nums[i], i + 1);
            // 如果存在满足条件的二元组，那么再加上 nums[i] 就是结果三元组
            for(List<Integer> t : temp){
                t.add(nums[i]);
                res.add(t);
            }
            // 跳过第一个数字重复的情况，否则会出现重复的结果。
            // 比如：nums = [-4, -1, -1, 0, 1, 2], target = 0。我们选了一次 -1 作为三元组的第一个元素后，下一次再选第一个元素时，第二个 -1 就需要跳过，否则就会出现重复结果
            while(i < nums.length - 1 && nums[i] == nums[i + 1]){
                i++;
            }
        }
        return res;
    }
    // 返回所有两数之和是 target 的元素对，且结果中不包含重复的二元组
    public List<List<Integer>> twoSumTarget(int[] nums, int target, int start){
        List<List<Integer>> res = new ArrayList<>();
        int low = start, high = nums.length - 1;
        while(low < high){
            int sum = nums[low] + nums[high];
            int left = nums[low], right = nums[high];
            if(sum < target){
                while(low < high && nums[low] == left){
                    low++;
                }
            }else if(sum > target){
                while(low < high && nums[high] == right){
                    high--;
                }
            }else{
                List<Integer> list = new ArrayList<>();
                list.add(nums[low]);
                list.add(nums[high]);
                res.add(new ArrayList(list));
                while(low < high && nums[low] == left){
                    low++;
                }
                while(low < high && nums[high] == right){
                    high--;
                }
            }
        }
        return res;
    }
}
```
* 时间复杂度：排序的时间复杂度是 $O(NlogN)$，twoSumTarget 函数中的双指针操作为 $O(N)$，threeSumTarget 函数在 for 循环中调用 twoSumTarget 函数，所以总的时间复杂度就是 $O( NlogN + N^2 ) = O (N^2)$
### 4Sum 问题
![图示](https://img-blog.csdnimg.cn/bac4f4b32bc14b08a00a5091839c8e26.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        for(int i = 0; i < nums.length; i++){
            List<List<Integer>> temp = threeSum(nums, target - nums[i], i + 1);
            // 如果存在满足条件的三元组，那么再加上 nums[i] 就是结果四元组
            for(List<Integer> t : temp){
                t.add(nums[i]);
                res.add(t);
            }
            while(i < nums.length - 1 && nums[i] == nums[i + 1]){
                i++;
            }
        }
        return res;
    }
    public List<List<Integer>> threeSum(int[] nums, int target, int start) {
        List<List<Integer>> res = new ArrayList<>();
        // 枚举三元组中的第一个数
        for(int i = start; i < nums.length; i++){
            List<List<Integer>> temp = twoSumTarget(nums, target - nums[i], i + 1);
            // 如果存在满足条件的二元组，那么再加上 nums[i] 就是结果三元组
            for(List<Integer> t : temp){
                t.add(nums[i]);
                res.add(t);
            }
            // 跳过数字重复的情况，否则会出现重复的结果。
            // 比如：nums = [-4, -1, -1, 0, 1, 2], target = 0。我们选了一次 -1 作为三元组的第一个元素后，下一次再选第一个元素时，第二个 -1 就需要跳过，否则就会出现重复结果
            while(i < nums.length - 1 && nums[i] == nums[i + 1]){
                i++;
            }
        }
        return res;
    }
    // 返回所有两数之和是 target 的元素对，且结果中没有出现重复
    public List<List<Integer>> twoSumTarget(int[] nums, int target, int start){
        List<List<Integer>> res = new ArrayList<>();
        int low = start, high = nums.length - 1;
        while(low < high){
            int sum = nums[low] + nums[high];
            int left = nums[low], right = nums[high];
            if(sum < target){
                while(low < high && nums[low] == left){
                    low++;
                }
            }else if(sum > target){
                while(low < high && nums[high] == right){
                    high--;
                }
            }else{
                List<Integer> list = new ArrayList<>();
                list.add(nums[low]);
                list.add(nums[high]);
                res.add(new ArrayList(list));
                while(low < high && nums[low] == left){
                    low++;
                }
                while(low < high && nums[high] == right){
                    high--;
                }
            }
        }
        return res;
    }
}
```
* 时间复杂度：排序的时间复杂度是 $O(NlogN)$，fourSumTarget 函数在 for 循环中调用 threeSumTarget 函数，所以总的时间复杂度就是 $O( NlogN + N^3 ) = O (N^3)$