

>给一个数组 nums=[5,4,8,2]，给一个 n=5416，让你从 nums 中选出一些元素，使得组成的数字是小于 n 的最大数，比如这个例子应该返回 5288

* 思路：
     * 如果可以追平（可以在 nums 数组中找到当前位的数），那么就追平
     * 如果不能追平（不能在 nums 数组中找到当前位的数），那么就尝试减小当前位：
     （2.1）如果当前位减小成功了（在 nums 数组中存在比当前位更小的数），那么后面的数就都是 nums 数组中的最大值就行
     （2.2）如果当前位减小失败了（在 nums 数组中没有比当前位更小的数了），那么就去操作当前位的前一位
```java
public class Main {
    public static void main(String[] args) {
        int[] nums = {2,6,8};
        System.out.println(getRes(nums, 222));
    }

    /*
     * 思路：
     *    1、如果可以追平（可以在 nums 数组中找到当前位的数），那么就追平
     *    2、如果不能追平（不能在 nums 数组中找到当前位的数），那么就尝试减小当前位:
     *       (2.1) 如果当前位减小成功了（在 nums 数组中存在比当前位更小的数），那么后面的数就都是 nums 数组中的最大值就行
     *       (2.2) 如果当前位减小失败了（在 nums 数组中没有比当前位更小的数了），那么就去操作当前位的前一位
     */
    public static int getRes(int[] nums, int n){
        if(n == 0) return -1; // 特殊情况

        Arrays.sort(nums);
        n--; // 因为要找的是小于 n 的最大值，也就是小于等于 n-1 的最大值

        // 将 n 的每一位的值放到数组中
        char[] chars = (n + "").toCharArray();
        int[] limit = new int[chars.length];
        for(int i = 0; i < chars.length; i++){
            limit[i] = chars[i] - 48;
        }

        // 得到和 limit 的长度一样长的最优解，如果没有那么就返回 -1
        int res = dfs(nums, limit, 0);
        if(res != -1){
            return res;
        }else{ // 比如：nums={2,6,8}, n=222，此时就会返回 -1，因为不存在小于等于 221 并且长度为 3 的最优解
            int len = limit.length - 1;
            if(len == 0) return -1; // 特殊情况
            int ans = 0;
            for(int i = 0; i < len; i++){
                ans = ans * 10 + nums[nums.length - 1];
            }
            return ans;
        }
    }

    public static int dfs(int[] nums, int[] limit, int index){
        // 如果所有位上的数在 nums 数组中都存在
        if(index == limit.length){
            int res = 0;
            for(int l : limit){
                res = res * 10 + l;
            }
            return res;
        }

        int temp = binarySearch(nums, limit[index]);
        // 如果在 nums 数组中没有比 limit[index] 更小的数了，那么就返回 -1，然后对当前位的前一位去做相应的操作
        if(temp == -1){
            return -1;
        }else if(nums[temp] == limit[index]){ // 如果可以在 nums 数组中找到 limit[index]
            int ans = dfs(nums, limit, index + 1);
            if(ans != -1){
                return ans;
            }else{
                if(temp > 0){ // 如果 nums 中存在比当前位要小的数
                    temp--; // 比当前位小的最大值的下标
                    // 将当前位变成比它小的最大值，然后将当前位后面的数都变成 nums 数组中的最大值
                    int res = 0;
                    for(int i = 0; i < index; i++){
                        res = res * 10 + limit[i];
                    }
                    res = res * 10 + nums[temp];
                    for(int i = index + 1; i < limit.length; i++){
                        res = res * 10 + nums[nums.length - 1];
                    }
                    return res;
                }else{
                    return -1;
                }
            }
        }else/* if(nums[temp] != limit[index])*/{ // 如果不能在 nums 数组中找到 limit[index]，但是 nums 数组中存在比 limit[index] 更小的数
            // 将当前位变成比它小的最大值，然后将当前位后面的数都变成 nums 数组中的最大值
            int res = 0;
            for(int i = 0; i < index; i++){
                res = res * 10 + limit[i];
            }
            res = res * 10 + nums[temp];
            for(int i = index + 1; i < limit.length; i++){
                res = res * 10 + nums[nums.length - 1];
            }
            return res;
        }
    }

    /*
      1、如果 nums 中存在 target，就返回其下标
      2、如果 nums 中不存在 target：
         (2.1) 如果 nums 中存在比 target 小的数，那么就返回比 target 小的数中的最大值的下标
         (2.2) 如果 nums 中不存在比 target 小的数，就返回 -1
    */
    public static int binarySearch(int[] nums, int target){
        int left = 0, right = nums.length - 1;
        int near = -1;
        while(left <= right){
            int mid = left + (right - left) / 2;
            if(nums[mid] == target){
                return mid;
            }else if(nums[mid] < target){
                near = mid;
                left = mid + 1;
            }else{
                right = mid - 1;
            }
        }
        return near;
    }
}
```