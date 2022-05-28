

343. 整数拆分
给定一个正整数 n，将其拆分为至少两个正整数的和，并使这些整数的乘积最大化。 返回你可以获得的最大乘积。
示例 1:
输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1。
示例 2:
输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36。
说明: 你可以假设 n 不小于 2 且不大于 58。

思路：
动态规划五部曲：
**1、确定dp数组以及下标的含义**
dp[i]：拆分数字 i，可以得到的最大乘积为dp[i]。
注：dp[i]的定义会贯穿整个题解过程，下面哪一步不懂就想想dp[i]代表的究竟是啥。
**2、确定递推公式**
可以想dp[i] (最大乘积)是怎么得到的呢?
其实可以从1遍历到 j，然后有两种渠道得到dp[i]。
j 的含义是正整数 i 拆分出来的第一个正整数是 j，然后剩余的部分就是 i - j。一种是剩余的部分再当成一个整体，一种是继续拆分。即：
一种是 j * (i - j)；
一种是 j * dp[i - j]，相当于是拆分了(i - j)。
那么为什么 j 不用拆分呢？
j 是从1开始遍历，拆分 j 的情况，在遍历 j 的过程dp[i - j]中其实都计算过了。
或者也可以理解为 j 是 i 拆分的第一个整数。
其实如果 j 也拆分了的话就没有3个数乘积的情况了。
那么从1遍历到 j，比较(i - j) * j和dp[i - j] * j取最大的。
递推公式：dp[i] = Math.max(dp[i], Math.max((i - j) * j, dp[i - j] * j));
**3、dp数组如何初始化**
严格从dp[i]的定义来说，dp[0]、dp[1]就不该初始化，也就是没有意义的数值。因此直接初始化dp[2] = 1，从dp[i]的定义来说，拆分数字2，得到的最大乘积是1。
**4、确定遍历顺序**
由递归公式，dp[i]是依靠dp[i - j]的状态，所以遍历 i 一定是从前向后遍历，先有dp[i- j]再有dp[i]。
枚举 j 的时候，是从1开始的。i是从3开始，这样dp[i - j]就是dp[2]正好可以通过我们初始化的值求出来。
**5、举例推导dp数组**
举例当n为10 的时候，dp数组里的数值，如下：
![图示](https://img-blog.csdnimg.cn/20210113160613305.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
class Solution {
    public int integerBreak(int n) {
        int[] dp = new int[n + 1];
        dp[2] = 1;
        for(int i = 3; i <= n; i++){
            for(int j = 1; j < i -  1; j++){
                dp[i] = Math.max(dp[i], Math.max((i - j) * j, dp[i - j] * j));
            }
        }
        return dp[n];
    }
}
```

以下代码也可以通过，在解释递推公式的时候，也可以解释通：dp[i]就等于拆解(i - j)的最大乘积(即dp[i - j]) * 拆解 j 的最大乘积(即dp[j])。
但在解释初始化的时候，就会自相矛盾，dp[1]为为什么是1？根据dp[i]的定义，dp[2]也不该是2。如果递推公式是dp[i - j] * dp[j]，这样就相当于强制把一个数至少拆分成四份：dp[i - j]至少是两个数的乘积，dp[i]也至少是两个数的乘积。但其实3以下的数，数的本身比任何它的拆分乘积都要大，所以初始化的时候才需要特殊处理。
因此如果递推公式是dp[i] = Math.max(dp[i], Math.max((i - j) * j, dp[i - j] * dp[j]))，就必须这么初始化。
```java
class Solution {
    public int integerBreak(int n) {
        if(n <= 3){
            return n - 1;
        }
        int[] dp = new int[n + 1];
        dp[1] = 1;
        dp[2] = 2;
        dp[3] = 3;
        for(int i = 4; i <= n; i++){
            for(int j = 1; j < i -  1; j++){
                dp[i] = Math.max(dp[i], Math.max((i - j) * j, dp[i - j] * dp[j]));
            }
        }
        return dp[n];
    }
}
```

后来做的笔记：

![图示](https://img-blog.csdnimg.cn/7d6429098d5e4233b21bd1cef9c0a8b3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
* 为什么不用拆分 j ？即只有 j * dp[i - j]，而没有 (i - j) * dp[j] ？
因为拆 j 得到的数字，拆 i - j 也能得到：
比如 i=5 时，j * dp[i - j] 分别为：1 * dp[4]，2 * dp[3]，3 * dp[2]，4 * dp[1]。当 j 为 1 时，虽然没有计算 dp[1] * 4，但在 j = 4 时计算的正是 dp[1] * 4 的情况。
```java
class Solution {
    public int integerBreak(int n) {
        int[] dp = new int[n + 1]; // 拆分数字 i，可以得到的最大乘积为 dp[i]
        dp[2] = 1;
        for(int i = 3; i <= n; i++){
            for(int j = 1; j < i; j++){
                // 拆分数字 i 所得的最大乘积有可能小于数字 i。比如：拆分 2 的最大乘积为 1，比 2 小
                dp[i] = Math.max(dp[i], Math.max(j * (i - j), j * dp[i - j]));
            }
        }
        return dp[n];
    }
}
```