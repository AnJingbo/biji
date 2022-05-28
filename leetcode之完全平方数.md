

279. 完全平方数
给定正整数 n，找到若干个完全平方数（比如 1, 4, 9, 16, ...）使得它们的和等于 n。你需要让组成和的完全平方数的个数最少。

给你一个整数 n ，返回和为 n 的完全平方数的 最少数量 。

完全平方数 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，1、4、9 和 16 都是完全平方数，而 3 和 11 不是。

示例 1：
输入：n = 12
输出：3 
解释：12 = 4 + 4 + 4
示例 2：
输入：n = 13
输出：2
解释：13 = 4 + 9

提示：1 <= n <= 104

思路：
将题目翻译一下：完全平方数就是物品（可以无限次使用），凑的正整数n就是背包容量，问凑满这个背包最少有多少物品？

动规五部曲：
**1、确定dp数组以及下标的含义**

dp[j]：**组成和为j的完全平方数的个数最少是dp[j]个**

**2、确定递推公式**

**如果你不使用第i个数**，也就是说你不使用i * i这个数，那么就继承之前的结果。

**如果你使用第i个数**，也就是说你使用i * i这个数，那么dp[j]应该等于dp[j-i * i] + 1。

dp[j]可以由dp[j - i * i]推出，dp[j - i * i] + 1便可以凑成dp[j]。

此时我们要选择最小的dp[j]，所以递推公式：dp[j] = min(dp[j], dp[j - i * i] + 1);

**3、dp数组如何初始化**

dp[0]表示组成和为0的完全平方数的最小数量，因此dp[0]一定是0。

从递推公式 dp[j] = min(dp[j], dp[j - i * i] + 1); 中可以看出，每次dp[j]都要选择最小的，**所以非0下标的dp[i]一定要初始为最大值，这样dp[j]在递推的时候才不会被初始值覆盖。**

**4、确定遍历顺序**

我们知道这是完全背包：

如果求组合数就是外层for循环遍历物品，内层for循环遍历背包；

如果求排列数就是外层for循环遍历背包，内层for循环遍历物品；

但本题是求完全平方数最小个数，那么有顺序和没有顺序都可以，都不影响它的最小个数。

所以**本题并不强调集合是组合还是排列**。

即本题的两个for循环的关系是：外层for循环遍历物品，内层for遍历背包或者外层for遍历背包，内层for循环遍历物品都是可以的！

**5、举例推到dp数组**
![图示](https://img-blog.csdnimg.cn/20210204105900683.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
先遍历物品再遍历背包：
```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1];//组成和为j的完全平方数的个数最少是dp[j]个        
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;
        for(int i = 1; i * i <= n; i++){
            for(int j = 1; j <= n; j++){
                if(j >= i * i && dp[j - i * i] != Integer.MAX_VALUE){
                /*if(j >= i * i) 也行，因为j - i * i这个数一定可以被完全平方数凑成(因为有1)，
                所以dp[j - i * i]一定不等于Integer.MAX_VALUE*/
                    dp[j] = Math.min(dp[j], dp[j - i * i] + 1);
                }
            }
        }
        return dp[n];
    }
}
```

```java
class Solution {
    public int numSquares(int n) {
        List<Integer> list = new ArrayList<>();
        for(int i = 1; i <= n; i++){
            if(i * i <= n){
                list.add(i * i);
            }
        }
        int[] nums = new int[list.size()];
        for(int i = 0; i < nums.length; i++){
            nums[i] = list.get(i);
        }

        int[] dp = new int[n + 1];//组成和为j的完全平方数的个数最少是dp[j]个        
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;
        for(int i = 0; i < nums.length; i++){
            for(int j = 1; j <= n; j++){
                if(j >= nums[i] && dp[j - nums[i]] != Integer.MAX_VALUE){
                /*if(j >= nums[i]) 也行，因为j - nums[i]这个数一定可以被完全平方数凑成(因为有1)，
                所以dp[j - nums[i]]一定不等于Integer.MAX_VALUE*/
                    dp[j] = Math.min(dp[j], dp[j - nums[i]] + 1);
                }
            }
        }
        return dp[n];
    }
}
```