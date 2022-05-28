

```java
/**
 * 347. 前 K 个高频元素
 * 给定一个非空的整数数组，返回其中出现频率前 k 高的元素。
 * <p>
 * 示例 :
 * 输入: nums = [1,1,1,2,2,3], k = 2
 * 输出: [1,2]
 */

import java.util.*;
//堆是一颗完全二叉树。如果父结点大于等于左右孩子就是大顶堆，小于等于左右孩子就是就是小顶堆
//优先队列出队就是删除堆顶结点
public class Ti30 {
    public static void main(String[] args) {
        int[] res = topKFrequent(new int[]{1, 1, 1, 2, 2, 3}, 2);
        System.out.println(Arrays.toString(res));
    }

    public static int[] topKFrequent(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap<>();
        int[] res = new int[k];
        //创建优先队列，并将其比较方式制定为比较value的大小（即某个元素出现的频率）
        Queue<Integer> queue = new PriorityQueue<>(new Comparator<Integer>() {
            public int compare(Integer a, Integer b) {
                return map.get(a) - map.get(b);//按value大小进行升序
                //return map.get(b) - map.get(a);//按value大小进行降序
            }
        });
        //将数组元素及其出现的频率存入HashMap中
        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(nums[i])) {
                map.put(nums[i], map.get(nums[i]) + 1);
            } else {
                map.put(nums[i], 1);
            }
        }
        //将key存入队列中（按照其对应的value的大小进行排序）
        for (int key : map.keySet()) {
            queue.add(key);
            if (queue.size() > k) {
                queue.poll();
            }
        }
        //将queue的值存入数组中
        for (int i = k - 1; i >= 0; i--) {
            res[i] = queue.poll();
        }
        return res;
    }
}
```