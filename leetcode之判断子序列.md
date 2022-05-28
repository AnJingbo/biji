

392. 判断子序列
给定字符串 s 和 t ，判断 s 是否为 t 的子序列。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，"ace"是"abcde"的一个子序列，而"aec"不是）。

进阶：
如果有大量输入的 S，称作 S1, S2, ... , Sk 其中 k >= 10亿，你需要依次检查它们是否为 T 的子序列。在这种情况下，你会怎样改变代码？ 

示例 1：
输入：s = "abc", t = "ahbgdc"
输出：true

示例 2：
输入：s = "axc", t = "ahbgdc"
输出：false

提示：
0 <= s.length <= 100
0 <= t.length <= 10^4
两个字符串都只由小写字符组成。

双指针法：
```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int i = 0, j = 0;
        while(i < s.length() && j < t.length()){
            if(s.charAt(i) == t.charAt(j)){
                i++;
            }
            j++;
        }
        return i == s.length();
        
    }
}
```
动规：

**1、确定dp数组（dp table）以及下标的含义**

dp[i][j] 表示**以下标i-1为结尾的字符串s，和以下标j-1为结尾的字符串t，相同子序列的长度为dp[i][j]。**

注意这里是判断s是否为t的子序列。即t的长度是大于等于s的。

**2、确定递推公式**

在确定递推公式的时候，首先要考虑如下两种操作，整理如下：

如果当前字符相等，说明 t 中找到了一个字符在 s 中也出现了；
如果当前字符不相等，相当于t要删除元素，继续匹配。

if (s.charAt(i - 1) == t.charAt(j - 1))，那么dp[i][j] = dp[i - 1][j - 1] + 1;，因为找到了一个相同的字符，相同子序列长度自然要在dp[i-1][j-1]的基础上加1。

if (s.charAt(i - 1) != t.charAt(j - 1))，此时相当于t要删除元素，即：dp[i][j] = dp[i][j - 1];

**3、dp数组如何初始化**

从递推公式可以看出dp[i][j]都是依赖于dp[i - 1][j - 1] 和 dp[i][j - 1]，dp[0][j]表示 空字符串"" 和 t字符串，dp[i][0]表示 s字符串 和 空字符串""。
```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int m = s.length();
        int n = t.length();
        int[][] dp = new int[m + 1][n + 1];//以下标i-1结尾的字符串s，和以下标j-1结尾的字符串t，相同子序列长度为dp[i][j]
        for(int i = 1; i <= m; i++){
            for(int j = 1; j <= n; j++){
                if(s.charAt(i - 1) == t.charAt(j - 1)){
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                }else{
                    dp[i][j] = dp[i][j - 1];
                }
            }
        }
        return dp[m][n] == m;
    }
}
```
求最长公共子序列，如果最长公共子序列的长度等于s的长度，那么s就是t的子序列
```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int m = s.length();
        int n = t.length();
        int[][] dp = new int[m + 1][n + 1];//长度为[0, i-1]的字符串s和长度为[0, j-1]的字符串t的最长公共子序列是dp[i][j]
        for(int i = 1; i <= m; i++){//求最长公共子序列
            for(int j = 1; j <= n; j++){
                if(s.charAt(i - 1) == t.charAt(j - 1)){
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                }else{
                    dp[i][j] = Math.max(dp[i][j - 1], dp[i - 1][j]);
                }
            }
        }
        return dp[m][n] == m;
    }
}
```