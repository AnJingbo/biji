

1143. 最长公共子序列
给定两个字符串 text1 和 text2，返回这两个字符串的最长公共子序列的长度。
一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。
例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。两个字符串的「公共子序列」是这两个字符串所共同拥有的子序列。
若这两个字符串没有公共子序列，则返回 0。

示例 1:
输入：text1 = "abcde", text2 = "ace" 
输出：3  
解释：最长公共子序列是 "ace"，它的长度为 3。
示例 2:
输入：text1 = "abc", text2 = "abc"
输出：3
解释：最长公共子序列是 "abc"，它的长度为 3。
示例 3:
输入：text1 = "abc", text2 = "def"
输出：0
解释：两个字符串没有公共子序列，返回 0。

提示:
1 <= text1.length <= 1000
1 <= text2.length <= 1000
输入的字符串只含有小写英文字符。

思路：
计算最长公共子序列（Longest Common Subsequence，简称 LCS）是一道经典的动态规划题目，大家应该都见过：

给你输入两个字符串s1和s2，请你找出他们俩的最长公共子序列，返回这个子序列的长度。

比如说输入s1 = "zabcde", s2 = "acez"，它俩的最长公共子序列是lcs = "ace"，长度为 3，所以算法返回 3。

如果没有做过这道题，一个最简单的暴力算法就是，把s1和s2的所有子序列都穷举出来，然后看看有没有公共的，然后在所有公共子序列里面再寻找一个长度最大的。

显然，这种思路的复杂度非常高，你要穷举出所有子序列，这个复杂度就是指数级的，肯定不实际。

正确的思路是不要考虑整个字符串，而是细化到s1和s2的每个字符。

**对于两个字符串求子序列的问题，都是用两个指针 i 和 j 分别在两个字符串上移动，大概率是动态规划思路。**

**第一步，一定要明确dp数组的含义**。

对于两个字符串的动态规划问题，套路是通用的。

比如说对于字符串 s1 = "ace" 和 s2 = "babcde",一般来说都要构造一个这样的 DP table：
![图示](https://img-blog.csdnimg.cn/20210316145021476.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
dp[i][j]的含义是：**对于s1[1..i]和s2[1..j]，它们的 LCS 长度是dp[i][j]**。

比如上图的例子，d[2][4] 的含义就是：对于"ac"和"babc"，它们的 LCS 长度是 2。我们最终想得到的答案应该是dp[3][6]。

**第二步，找状态转移方程。**

这是动态规划最难的一步，不过好在这种字符串问题的套路都差不多，权且借这道题来聊聊处理这类问题的思路。

状态转移说简单些就是做选择，比如说这个问题，是求s1和s2的最长公共子序列，不妨称这个子序列为lcs。那么对于s1和s2中的每个字符，有什么选择？很简单，两种选择，要么在lcs中，要么不在。

这个「在」和「不在」就是选择，关键是，应该如何选择呢？这个需要动点脑筋：**如果s1[i] == s2[j]，说明这个字符一定在lcs中；**

**如果s1[i] != s2[j]，说明s1[i]和s2[j]至少有一个字符不在lcs中：**

* 情况1：s1[i]不在lcs中，比如: "abe" 和 "acb";

* 情况2：s2[j]不在lcs中，比如: "abc" 和 "ace";

* 情况3：s1[i]和s2[j]都不在lcs中，比如: "abm" 和 "acn"

在这三种情况中取最大值。但其实第三种情况不需要考虑，因为它已经被第一种和第二种情况包含了。比如："abc"和"ace"，c 和 e 不相等，那么对于情况1就是 "ab" 和"ace"，对于情况2就是 "abc" 和"ac"，对于情况3就是 "ab" 和"ac"，可见情况1和情况2都包含了情况3，即情况1和情况2的值一定大于等于情况三的值。

**第三步，如何初始化。**

我们专门**让索引为 0 的行和列表示空串**，dp[0][..]和dp[..][0]都应该初始化为 0，这就是 base case。

比如说，按照刚才 dp 数组的定义，dp[0][3]=0的含义是：对于字符串""和"bab"，其 LCS 的长度为 0。因为有一个字符串是空串，它们的最长公共子序列的长度显然应该是 0。

动规写法：
```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length();
        int n = text2.length();
        int[][] dp = new int[m + 1][n + 1];
        for(int i = 1; i <= m; i++){
            for(int j = 1; j <= n; j++){
                if(text1.charAt(i - 1) == text2.charAt(j - 1)){//找到一个lcs中的字符
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

递归：用两个指针i和j从后往前遍历s1和s2，如果s1[i]==s2[j]，那么这个字符一定在lcs中；否则的话，s1[i]和s2[j]这两个字符至少有一个不在lcs中，需要丢弃一个。

```java
class Solution {
    String s1;
    String s2;
    public int longestCommonSubsequence(String text1, String text2) {
        s1 = text1;
        s2 = text2;
        return dp(s1.length() - 1, s2.length() - 1);
    }
    public int dp(int i, int j){
        if(i == -1 || j == -1){//空串的 base case
            return 0;
        }
        if(s1.charAt(i) == s2.charAt(j)){//找到一个 lcs 的元素，继续往前找
            return dp(i - 1, j - 1) + 1;
        }
        return Math.max(dp(i - 1, j), dp(i, j - 1));//谁能让 lcs 最长，就听谁的
    }
}
```
对于第一种情况，找到一个lcs中的字符，同时将i, j向前移动一位，并给lcs的长度加一；对于后者，则尝试两种情况，取更大的结果。