

```java
/**
 * 示例 1：
 * 输入：arr = [40,10,20,30]
 * 输出：[4,1,2,3]
 * 解释：40 是最大的元素。 10 是最小的元素。 20 是第二小的数字。 30 是第三小的数字。
 * 示例 2：
 * 输入：arr = [100,100,100]
 * 输出：[1,1,1]
 * 解释：所有元素有相同的序号。
 * 示例 3：
 * 输入：arr = [37,12,28,9,100,56,80,5,12]
 * 输出：[5,3,4,2,8,6,7,1,3]
 * <p>
 * <p>
 * leetcode: 1331
 * https://leetcode-cn.com/problems/rank-transform-of-an-array/
 */

import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

public class Ti18 {
    public static void main(String[] args) {
        int[] arr = {40, 10, 20, 30};
        int[] res = arrayRankTransform(arr);
        System.out.println(Arrays.toString(res));
    }

    public static int[] arrayRankTransform(int[] arr) {
        int min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
        //找最大最小值
        for (int num : arr) {
            if (num < min) min = num;
            if (num > max) max = num;
        }
        int[] count = new int[max - min + 1];//最多存在 max-min+1 个不同元素
        //初次遍历，找到存在的元素
        for (int num : arr) count[num - min] = 1;
        //二次遍历，对存在的元素进行排序
        for (int i = 1; i < count.length; i++) {
            count[i] = count[i - 1] + count[i];
        }
        int[] res = new int[arr.length];
        for (int i = 0; i < arr.length; i++) {
            res[i] = count[arr[i] - min];//count数组存放着对应的排序后索引
        }
        return res;
    }

    public static int[] arrayRankTransform1(int[] arr) {
        int[] res = new int[arr.length];
        int index = 1;
        for (int i = 0; i < res.length; i++) {
            res[i] = arr[i];
        }
        Arrays.sort(arr);
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < arr.length; i++) {
            if (i == 0) {
                map.put(arr[i], index);
                index++;
            }
            if (i > 0 && arr[i] != arr[i - 1]) {
                map.put(arr[i], index);
                index++;
            }
        }

        for (int i = 0; i < res.length; i++) {
            res[i] = map.get(res[i]);
        }
        return res;
    }
}
```