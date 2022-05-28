

![图示](https://img-blog.csdnimg.cn/2021033014552611.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
**1、确定dp数组（dp table）以及下标的含义**

dp[i][j]：长度为i的字符串s的子序列中出现长度为j的字符串t的个数为dp[i][j]。

**2、确定递推公式**

**这一类问题，基本是要分析两种情况：
s.charAt(i - 1) == t.charAt(j - 1)
s.charAt(i - 1) != t.charAt(j - 1)**

当 s.charAt(i - 1) 与 t.charAt(j - 1) 相等时，dp[i][j]可以有两部分组成：

一部分是用s.charAt(i - 1)来匹配，那么个数为dp[i - 1][j - 1]。

一部分是不用s.charAt(j - 1)来匹配，个数为dp[i - 1][j]。

为什么还要考虑不用s[i - 1]来匹配，都相同了肯定是要匹配的啊。

例如：s = "bagg" 和 t = "bag" ，s.charAt(3) 和 t.charAt(2) 是相等的，那么可以用s.charAt(3)来匹配，即s.charAt(0)、s.charAt(1)、s.charAt(3)组成的bag。

也可以不用s.charAt(3)来匹配，即用s.charAt(0)、s.charAt(1)、s.charAt(2)组成的bag。

所以当s.charAt(i - 1) 与 t.charAt(j - 1)相等时，dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];

当s.charAt(i - 1) 与 t.charAt(j - 1)不相等时，dp[i][j]只由一部分组成，不用s.charAt(i - 1)来匹配，即：dp[i - 1][j]

所以递推公式为：dp[i][j] = dp[i - 1][j];

**3、dp数组如何初始化**

从递推公式dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]; 和 dp[i][j] = dp[i - 1][j]; 中可以看出dp[i][0] 和dp[0][j]是一定要初始化的。

每次当初始化的时候，都要回顾一下dp[i][j]的定义，不要凭感觉初始化。

dp[i][0]一定都是1，因为s字符串变成""，删除所有元素，出现空字符串的个数就是1。

dp[0][j]一定都是0，因为""无论如何也变成不了字符串t。

最后就要看一个特殊位置了，即：dp[0][0] 应该是多少。

dp[0][0]应该是1，空字符串s，可以删除0个元素(也可以理解为不删除元素，将不删除元素也看成一种方案)，变成空字符串t。

![图示](https://img-blog.csdnimg.cn/20210330152038932.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
class Solution {
    public int numDistinct(String s, String t) {
        int m = s.length(), n = t.length();
        int[][] dp = new int[m + 1][n + 1];//长度为i的字符串s变成长度为j的字符串t的方案数
        for(int i = 0; i <= m; i++){
            dp[i][0] = 1;//别的字符串变成空字符串都是只有1种方案
        }
        for(int i = 1; i <= m; i++){
            for(int j = 1; j <= n; j++){
                if(i < j){//如果i小于j，那么说明s字符串长度比t字符串长度小，那么s怎么也变不成t
                    break;
                }
                if(s.charAt(i - 1) == t.charAt(j - 1)){
                    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
                }else{
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        return dp[m][n];
    }
}
```
自己写这道题的时候刚开始没想出状态转移方程，但是先把dp数组画出来了，然后照着dp数组写出了状态转移方程。。。。。。