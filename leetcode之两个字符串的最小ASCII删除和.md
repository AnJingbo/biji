

712. 两个字符串的最小ASCII删除和
给定两个字符串s1, s2，找到使两个字符串相等所需删除字符的ASCII值的最小和。

示例 1:
输入: s1 = "sea", s2 = "eat"
输出: 231
解释: 在 "sea" 中删除 "s" 并将 "s" 的值(115)加入总和。
在 "eat" 中删除 "t" 并将 116 加入总和。
结束时，两个字符串相等，115 + 116 = 231 就是符合条件的最小和。

示例 2:
输入: s1 = "delete", s2 = "leet"
输出: 403
解释: 在 "delete" 中删除 "dee" 字符串变成 "let"，
将 100[d]+101[e]+101[e] 加入总和。在 "leet" 中删除 "e" 将 101[e] 加入总和。
结束时，两个字符串都等于 "let"，结果即为 100+101+101+101 = 403 。
如果改为将两个字符串转换为 "lee" 或 "eet"，我们会得到 433 或 417 的结果，比答案更大。

注意:
0 < s1.length, s2.length <= 1000。
所有字符串中的字符ASCII值在[97, 122]之间。

**一、确定dp数组及其下标的含义**

dp[i][j]：字符串s1[1...i]和字符串s2[1...j]要想相同所需要删除字符的ASCII值的最小和是dp[i][j]。

**二、确定递推公式**

1、当 s1[i - 1] 与 s2[j - 1] 相等的时候，dp[i][j] = dp[i - 1][j - 1];

2、当 s1[i - 1] 与 s2[j - 1] 不相同的时候，有三种情况：

删除 s1[i - 1]，删除字符的ASCII值的最小和为 dp[i - 1][j] + s1.charAt(i - 1);
删除 s2[i - 1]，删除字符的ASCII值的最小和为 dp[i][j - 1]) + s2.charAt(j - 1);
同时删除 s1[i - 1] 和 s2[i - 1]，删除字符的ASCII值的最小和为 dp[i - 1][j - 1] + s1.charAt(i - 1) + s2.charAt(j - 1)

即：dp[i][j] = min(dp[i - 1][j - 1] + s1.charAt(i - 1) + s2.charAt(j - 1),
 min(dp[i - 1][j] + s1.charAt(i - 1), dp[i][j - 1]) + s2.charAt(j - 1));

**三、如何初始化**

当 i 为0时，表示s1为空串；当 j 为0时，表示s2为空串。

因此dp[0][0]一定是0，因为空串转化为空串不需要删除任何字符。

其余比如："" 和 "ea"，则dp[0][2] = 'e' + 'a' = 198; 
"se" 和 ""，则dp[2][0] = 's' + 'e' = 216;
```java
class Solution {
    public int minimumDeleteSum(String s1, String s2) {
        int m = s1.length();
        int n = s2.length();
        int[][] dp = new int[m + 1][n + 1];//字符串s1[1...i]和字符串s2[1...j]要想相同所需要删除字符的ASCII值的最小和是dp[i][j]
        for(int i = 1; i <= m; i++){
            dp[i][0] = dp[i - 1][0] + s1.charAt(i - 1);
        }
        for(int i = 1; i <= n; i++){
            dp[0][i] = dp[0][i - 1] + s2.charAt(i - 1);
        }
        for(int i = 1; i <= m; i++){
            for(int j = 1; j <= n; j++){
                if(s1.charAt(i - 1) == s2.charAt(j - 1)){
                    dp[i][j] = dp[i - 1][j - 1];
                }else{
                    dp[i][j] = Math.min(dp[i - 1][j - 1] + s1.charAt(i - 1) + s2.charAt(j - 1),
                    Math.min(dp[i - 1][j] + s1.charAt(i - 1), dp[i][j - 1] + s2.charAt(j - 1)));
                }
            }
        }
        return dp[m][n];
    }
}
```