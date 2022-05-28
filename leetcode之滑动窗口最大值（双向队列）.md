

```java
/**
 * 239. 滑动窗口最大值
 * 给定一个数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。
 * 滑动窗口每次只向右移动一位, 返回滑动窗口中的最大值。
 */


import java.util.Arrays;
import java.util.Deque;
import java.util.LinkedList;

public class Ti29 {
    public static void main(String[] args) {
        int[] res = maxSlidingWindow(new int[]{1,3,-1,-3,5,3,6,7}, 3);
        System.out.println(Arrays.toString(res));
    }
    public static int[] maxSlidingWindow(int[] nums, int k) {
        int[] res = new int[nums.length - k + 1];
        MyQueue mq = new MyQueue();
        for (int i = 0; i < k; i++) {
            mq.add(nums[i]);
        }
        res[0] = mq.peek();
        for (int i = k, t = 1; i < nums.length; i++, t++) {
            mq.poll(nums[i - k]);
            mq.add(nums[i]);
            res[t] = mq.peek();
        }
        return res;
    }
}

class MyQueue {
    Deque<Integer> deque = new LinkedList<>();

    public void add(int val) {
        while (!deque.isEmpty() && val > deque.getLast()) {
            deque.pollLast();
        }
        deque.addLast(val);
    }

    public void poll(int val) {
        if (!deque.isEmpty() && val == deque.peek()) {
            deque.poll();
        }
    }

    public int peek() {
        return deque.peek();
    }
}
```