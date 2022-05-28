

583. 两个字符串的删除操作
给定两个单词 word1 和 word2，找到使得 word1 和 word2 相同所需的最小步数，每步可以删除任意一个字符串中的一个字符。

示例：
输入: "sea", "eat"
输出: 2
解释: 第一步将"sea"变为"ea"，第二步将"eat"变为"ea"

提示：
给定单词的长度不超过500。
给定单词中的字符只含有小写字母。

方法一：

**一、确定dp数组及其下标含义**

dp[i][j]：字符串word1[1 ... i]和字符串word2[1 ... j]，要想相等所需要的最小删除次数是dp[i][j]。

**二、确定递推公式**

1、当 word1[i - 1] 与 word2[j - 1] 相等的时候，dp[i][j] = dp[i - 1][j - 1];

2、当 word1[i - 1] 与 word2[j - 1] 不相同的时候，有三种情况：

删除 word1[i - 1]，最少操作次数为 dp[i - 1][j] + 1
删除 word2[i - 1]，最少操作次数为 dp[i][j - 1] + 1
同时删除 word1[i - 1] 和 word2[i - 1]，操作的最少次数为 dp[i - 1][j - 1] + 2

即：dp[i][j] = min(dp[i - 1][j - 1] + 2, min(dp[i - 1][j], dp[i][j - 1]) + 1);

**三、dp数组如何初始化**

当 i 为0时，表示word1为空串；当 j 为0时，表示word2为空串。

因此dp[i][0]应该全部初始化为i，dp[0][j]应该全部初始化为j。

比如：dp[0][2]表示 "" 和 “se” 要想相同所需要的最小删除次数是2；dp[3][0]表示 "sea" 和 “” 要想相同所需要的最小删除次数是3

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();
        int[][] dp = new int[m + 1][n + 1];//字符串word1[1 … i]和字符串word2[1 … j]，要想相等所需要的最小删除次数是dp[i][j]
        for(int i = 0; i <= n; i++){
            dp[0][i] = i;
        }
        for(int i = 0; i <= m; i++){
            dp[i][0] = i;
        }

        for(int i = 1; i <= m; i++){
            for(int j = 1; j <= n; j++){
                if(word1.charAt(i - 1) == word2.charAt(j - 1)){
                    dp[i][j] = dp[i - 1][j - 1];
                }else {
                    dp[i][j] = Math.min(dp[i - 1][j - 1] + 2, Math.min(dp[i - 1][j] + 1, dp[i][j - 1] + 1));
                }
            }
        }
        return dp[m][n];
    }
}
```

方法二：

删除后的结果其实就是两个字符串的最长公共子序列！

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();
        int len = longestCommonSubsequence(word1, word2);
        return m - len + n - len;
    }
    public int longestCommonSubsequence(String text1, String text2){//返回最长公共子序列的长度
        int m = text1.length();
        int n = text2.length();
        int[][] dp = new int[m + 1][n + 1];
        for(int i = 1; i <= m; i++){
            for(int j = 1; j <= n; j++){
                if(text1.charAt(i - 1) == text2.charAt(j - 1)){
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                }else{
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[m][n];
    }
}
```