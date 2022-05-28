

509. 斐波那契数
斐波那契数，通常用 F(n) 表示，形成的序列称为 斐波那契数列 。该数列由 0 和 1 开始，后面的每一项数字都是前面两项数字的和。也就是：
F(0) = 0，F(1) = 1
F(n) = F(n - 1) + F(n - 2)，其中 n > 1
给你 n ，请计算 F(n) 。
示例 1：
输入：3
输出：2
解释：F(3) = F(2) + F(1) = 1 + 1 = 2
示例 2：
输入：4
输出：3
解释：F(4) = F(3) + F(2) = 2 + 1 = 3
提示：0 <= n <= 30

思路：
动规五部曲：
**1、确定dp数组以及下标的含义**
dp[i]的定义为：第 i 个数的斐波那契值是dp[i]
**2、确定递推公式**
题目已经把递推公式给出了：dp[i] = dp[i - 1] + dp[i - 2];
**3、dp公式如何初始化**
题目中也已经把如何初始化也直接给我们了：
dp[0] = 0; dp[1] = 1;
**4、确定遍历顺序**
从递归公式 dp[i] = dp[i - 1] + dp[i - 2];中可以看出，dp[i]是依赖dp[i - 1]和dp[i - 2]，那么遍历顺序一定是从前到后遍历的
**5、举例推导dp数组**
按照这个递推公式dp[i] = dp[i - 1] + dp[i - 2]，我们来推导一下，当n为10的时候，dp数组应该是如下的数列：
0 1 1 2 3 5 8 13 21 34 55
如果代码写出来，发现结果不对，就把dp数组打印出来看看和我们推导出来的数列是不是一致的。
自己写的：
```java
class Solution {
    public int fib(int n) {
        int[] dp = new int[31];
        dp[0] = 0;
        dp[1] = 1;
        for(int i = 2; i < 31; i++){
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
}
```
优化一下：
```java
class Solution {
    public int fib(int n) {
        if(n <= 1){
            return n;
        }
        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = 1;
        for(int i = 2; i <= n; i++){
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
}
```
其实只需要维护两个数值即可，不需要记录整个序列：
```java
class Solution {
    public int fib(int n) {
        if(n <= 1){
            return n;
        }
        int[] dp = new int[2];
        dp[0] = 0;
        dp[1] = 1;
        for(int i = 2; i <= n; i++){
            int sum = dp[0] + dp[1];
            dp[0] = dp[1];
            dp[1] = sum;
        }
        return dp[1];
    }
}
```