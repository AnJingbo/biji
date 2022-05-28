

![图示](https://img-blog.csdnimg.cn/eb21b8ac7e184fd9b6cf7df03554e537.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
### 暴力解法
```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int res = 0;
        for(int i = 0; i < nums.length; i++){
            int sum = 0;
            for(int j = i; j < nums.length; j++){
                sum += nums[j];
                if(sum == k){
                    res++;
                }
            }
        }
        return res;
    }
}
```
```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int n = nums.length;
        int[] preSum = new int[n + 1]; // 前缀和数组。preSum[i] 存储 nums[0 ... i-1] 的和，即 nums 前 i 个元素的和
        preSum[0] = 0;
        for(int i = 1; i <= n; i++){
            preSum[i] = preSum[i - 1] + nums[i - 1];
        }
        int res = 0;
        for(int i = 0; i <= n; i++){
            for(int j = i + 1; j <= n; j++){
                if(preSum[j] - preSum[i] == k){
                    res++;
                }
            }
        }
        return res;
    }
}
```
### 前缀和
```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int n = nums.length;
        int[] preSum = new int[n + 1]; // 前缀和数组。preSum[i] 存储 nums[0 ... i-1] 的和，即 nums 前 i 个元素的和
        preSum[0] = 0;
        for(int i = 1; i <= n; i++){
            preSum[i] = preSum[i - 1] + nums[i - 1];
        }
        int res = 0;
        // 前缀和 -> 该前缀和出现的次数
        HashMap<Integer, Integer> map = new HashMap<>();
        map.put(0, 1); // 必须有这行。比如：nums=[1,1,1],k=2
        for(int i = 1; i <= n; i++){
            int target = preSum[i] - k;
            if(map.containsKey(target)){
                res += map.get(target);
            }
            map.put(preSum[i], map.getOrDefault(preSum[i], 0) + 1);
        }
        return res;
    }
}
```
```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int n = nums.length;
        int res = 0;
        int sum = 0; // 用于计算前缀和

        // 前缀和 -> 该前缀和出现的次数
        HashMap<Integer, Integer> map = new HashMap<>();
        map.put(0, 1); // 必须有这行。比如：nums=[1,1,1],k=2
        for(int i = 0; i < n; i++){
            sum += nums[i];
            int target = sum - k;
            if(map.containsKey(target)){
                res += map.get(target);
            }
            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
        return res;
    }
}
```