

### 31. 下一个排列
![ts](https://img-blog.csdnimg.cn/c55785bea2924d57ba2fed0f62b81c5c.png)
```java
/*
对于 [2, 6, 3, 5, 4, 1]
  (1)从后往前找到一个逆序数 N, 即为 3
  (2)然后再从 N 后面找到一个比它大的"最小数"。即在 [5, 4, 1] 中找到比 3 大的最小数, 为 4（这里可以直接从后往前找，因为 N 后面的数字倒着遍历一定是正序的）
  (3)交换两者位置，则为 [2, 6, 4, 5, 3, 1]
  (4)然后对一开始 N 后面位置进行反转，即从 [2, 6, 4, 5, 3, 1] 到 [2, 6, 4, 1, 3, 5]
*/
class Solution {
    public void nextPermutation(int[] nums) {
        int i = nums.length - 2;
        while(i >= 0 && nums[i] >= nums[i + 1]){
            i--;
        }
        if(i < 0){
            reverse(nums, 0, nums.length - 1);
            return;
        }
        int j = nums.length - 1;
        while(j > i && nums[j] <= nums[i]){
            j--;
        }
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
        reverse(nums, i + 1, nums.length - 1);
    }
    public void reverse(int[] nums, int start, int end){
        while(start < end){
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
}
```