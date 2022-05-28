

```java
/**
 * 15. 三数之和
 * <p>
 * 给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。
 * 注意：答案中不可以包含重复的三元组。
 * <p>
 * 示例：
 * 给定数组 nums = [-1, 0, 1, 2, -1, -4]
 * 满足要求的三元组集合为：
 * [
 * [-1, 0, 1],
 * [-1, -1, 2]
 * ]
 */

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Ti23 {
    public static void main(String[] args) {
        List<List<Integer>> res = threeSum(new int[]{-1, 0, 1, 2, -1, -4});
        //List<List<Integer>> res = threeSum(new int[]{0, 0, 0, 0});
        for (List<Integer> list : res) {
            System.out.println(list);
        }
    }

    public static List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(nums);
        for (int i = 0; i < nums.length - 2; i++) {
            //排序之后如果第一个元素已经大于0，那么无论怎么组合都不可能凑成三元组
            if (nums[i] > 0) {
                break;
            }
            //错误去重方法，比如会漏掉-1，-1，2这种情况
            /*if(nums[i] == nums[i + 1]){
                continue;
            }*/
            //去重
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int left = i + 1;
            int right = nums.length - 1;
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum == 0) {
                    List<Integer> list = new ArrayList<>();
                    list.add(nums[i]);
                    list.add(nums[left]);
                    list.add(nums[right]);
                    res.add(list);
                    //去重，去重逻辑应该放在找到一个三元组之后
                    while (left < right && nums[right] == nums[right - 1]) {
                        right--;
                    }
                    while (left < right && nums[left] == nums[left + 1]) {
                        left++;
                    }
                    //找到答案时，双指针同时收缩
                    right--;
                    left++;
                } else if (sum > 0) {//如果sum>0，说明三数之和大了，需要左移right使和变小
                    right--;
                } else {//如果sum<0，说明三数之和小了，需要右移left使和变大
                    left++;
                }
            }
        }
        return res;
    }
}
```