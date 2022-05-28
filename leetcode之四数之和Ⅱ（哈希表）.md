

```java
/**
 * 454. 四数相加 II
 * 给定四个包含整数的数组列表 A , B , C , D ,计算有多少个元组 (i, j, k, l) ，使得 A[i] + B[j] + C[k] + D[l] = 0。
 * 为了使问题简单化，所有的 A, B, C, D 具有相同的长度 N，且 0 ≤ N ≤ 500 。所有整数的范围在 -228 到 228 - 1 之间，最终结果不会超过 231 - 1 。
 * 例如:
 * 输入:
 * A = [ 1, 2]
 * B = [-2,-1]
 * C = [-1, 2]
 * D = [ 0, 2]
 * 输出:
 * 2
 * 解释:
 * 两个元组如下:
 * 1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
 * 2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0
 */

import java.util.HashMap;

public class Ti22 {
    public static void main(String[] args) {
        int[] a = {1, 2};
        int[] b = {-2, -1};
        int[] c = {-1, 2};
        int[] d = {0, 2};
        int res = fourSumCount(a, b, c, d);
        System.out.println(res);
    }

   public static int fourSumCount(int[] a, int[] b, int[] c, int[] d) {
        int len = a.length;
        int res = 0;
        HashMap<Integer, Integer> map = new HashMap<>();
        //map中key存放两个元素之和，value存放这个和出现的次数
        for (int i = 0; i < len; i++) {
            for (int j = 0; j < len; j++) {
                int sum = a[i] + b[j];
                if (map.containsKey(sum)) {
                    map.put(sum, map.get(sum) + 1);
                } else map.put(sum, 1);
            }
        }
        for (int i = 0; i < len; i++) {
            for (int j = 0; j < len; j++) {
                int sum = c[i] + d[j];
                if (map.containsKey(-sum)) {
                    res += map.get(-sum);
                }
            }
        }
        return res;
    }
}
```