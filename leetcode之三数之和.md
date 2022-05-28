

![ts](https://img-blog.csdnimg.cn/68723529b2854e4aa760635b96215562.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        for(int i = 0; i < nums.length; i++){
            List<List<Integer>> temp = twoSumTarget(nums, 0 - nums[i], i + 1);
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
    // 返回所有两数之和是 target 的元素对，并且结果中不能出现相同的元素对
    public List<List<Integer>> twoSumTarget(int[] nums, int target, int start){
        List<List<Integer>> res = new ArrayList<>();
        int low = start, high = nums.length - 1;
        while(low < high){
            int sum = nums[low] + nums[high];
            int left = nums[low], right = nums[high];
            if(sum < target){
                low++;
            }else if(sum > target){
                high--;
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

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        for(int i = 0; i < nums.length; i++){
            List<List<Integer>> temp = twoSumTarget(nums, 0 - nums[i], i + 1);
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