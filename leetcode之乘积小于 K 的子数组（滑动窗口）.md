

### 713. 乘积小于 K 的子数组
![ts](https://img-blog.csdnimg.cn/e9fcf9d426604bb2b6ec18faf86838bc.png)
```java
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        int left = 0, right = 0;
        int temp = 1;
        int res = 0;
        // 以每个数字为右边界所形成的有效子数组的个数 
        while(right < nums.length){
            int num1 = nums[right];
            right++;
            temp *= num1;
            // 必须有 left<right 这一判断。比如：nums=[1,2,3],k=0
            while(left < right && temp >= k){
                int num2 = nums[left];
                left++;
                temp /= num2;
            }
            res += right - left;
        }
        return res;
    }
}
```