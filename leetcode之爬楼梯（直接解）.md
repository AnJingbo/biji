

70. 爬楼梯
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
注意：给定 n 是一个正整数。
示例 1：
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1 阶 + 1 阶； 2 阶
示例 2：
输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1 阶 + 1 阶 + 1 阶；1 阶 + 2 阶；2 阶 + 1 阶

思路：
本题多举几个例子，就可以发现其中规律。
爬到第一层楼梯有一种方法，爬到第二层楼梯有两种方法。
那么第一层楼梯再跨两步就到第三层，第二层楼梯再跨一步就到第三层。
所以到第三层楼梯的状态可以由第二层楼梯和到第一层楼梯的状态推导出来，那么就可以想到动态规划了。
动规五部曲：
定义一个一维数组来表示不同楼层的状态
**1、确定dp数组以及下标的含义**
dp[i]：爬到第 i 层楼梯，有dp[i]种方法
**2、确定递推公式**
如何可以推出dp[i]呢？
从dp[i]的定义可以看出，dp[i]可以有两个方向推出来：
首先是dp[i - 1]，上 i - 1 层楼梯，有dp[i - 1]种方法，那么再一步跳一个台阶就是dp[i]了；
还有就是dp[i - 2]，上 i - 2 层楼梯，有dp[i - 2]种方法，那么再一步跳两个台阶就是dp[i]了。
那么dp[i]就是dp[i - 1]和dp[i - 2]之和。
所以dp[i] = dp[i - 1] + dp[i - 2]。
在推到dp[i]的时候，一定要时刻想着dp[i]的定义，否则容易跑偏。
**3、dp数组如何初始化**
回顾一下dp[i]的定义：爬到第 i 层楼梯，有dp[i]种方法。
那么 i 为0时，dp[i]应该是多少呢，本题其实本质上不需要考虑dp[0]的初始化，只初始化dp[1] = 1、dp[2] = 2，然后从i = 3开始递推，因为题目说了n是一个正整数，没有说n为0的情况。
**4、确定遍历顺序**
从递推公式dp[i] = dp[i - 1] + dp[i - 2];中可以看出，遍历顺序一定是从前向后遍历的。
**5、举例推导dp数组**
举例当n为5的时候，dp table(dp数组)应该如下：
![图示](https://img-blog.csdnimg.cn/20210106162745196.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
如果代码出现问题，就把dp table打印出来，看看究竟是不是和自己推导的一样。
由此可见：本题其实就是斐波那契数列，只不过本题没有讨论dp[0]应该是什么，因为dp[0]在本题没有意义。
```java
class Solution {
    public int climbStairs(int n) {
        if(n <= 2){
            return n;
        }
        int[] dp = new int[n + 1];
        dp[1] = 1;
        dp[2] = 2;
        for(int i = 3; i < dp.length; i++){
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
}
```

优化空间复杂度：
```java
class Solution {
    public int climbStairs(int n) {
        if(n <= 2){
            return n;
        }
        int[] dp = new int[3];
        dp[1] = 1;
        dp[2] = 2;
        for(int i = 3; i <= n; i++){
            int sum = dp[1] + dp[2];
            dp[1] = dp[2];
            dp[2] = sum;
        }
        return dp[2];
    }
} 
```