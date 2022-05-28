

```java
import java.util.Arrays;
import java.util.HashMap;

/**
 * 1、两数之和
 * 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
 * 你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。
 * <p>
 * 示例:
 * 给定 nums = [2, 7, 11, 15], target = 9
 * 因为 nums[0] + nums[1] = 2 + 7 = 9
 * 所以返回 [0, 1]
 * <p>
 * 链接：https://leetcode-cn.com/problems/two-sum
 */
public class Ti21 {
    public static void main(String[] args) {
        //int[] res = twoSum(new int[]{2, 7, 11, 15}, 9);
        int[] res = twoSum(new int[]{3, 2, 4}, 6);
        System.out.println(Arrays.toString(res));
    }

    public static int[] twoSum(int[] nums, int target) {
        int[] a = new int[2];
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            map.put(nums[i], i);
        }
        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(target - nums[i])) {
                a[0] = i;
                a[1] = map.get(target - nums[i]);
                //这一步要有，否则可能会返回的数组的值相同
                if (a[0] != a[1]) {
                    return a;
                }
            }
        }
        return new int[0];
    }

    public static int[] twoSum1(int[] nums, int target) {
        int[] a = new int[2];
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(target - nums[i])) {
                a[0] = map.get(target - nums[i]);
                a[1] = i;
                return a;
            }
            map.put(nums[i], i);
        }
        return new int[0];
    }
}
```