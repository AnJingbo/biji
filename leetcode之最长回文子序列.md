

516. 最长回文子序列
给定一个字符串 s ，找到其中最长的回文子序列，并返回该序列的长度。可以假设 s 的最大长度为 1000 。

示例 1:
输入: "bbbab"
输出: 4
一个可能的最长回文子序列为 "bbbb"。

示例 2:
输入: "cbbd"
输出: 2
一个可能的最长回文子序列为 "bb"。

提示：
1 <= s.length <= 1000
s 只包含小写英文字母

**1、确定dp数组（dp table）以及下标的含义**

dp[i][j]：**字符串s在区间[i, j]范围内最长的回文子序列的长度为dp[i][j]。注意：因为是区间，所以本题隐含了的条件是：j>=i，因此下面的第二层循环中的 j 是从 i 开始的。**

**2、确定递推公式**

**如果s.charAt(i)与s.charAt(j)不相等**，那么说明s.charAt(i)和s.charAt(j)的同时加入并不能增加[i, j]区间内的回文子序列的长度，因此就需要**分别加入s.charAt(i)和s.charAt(j)来看看哪一个可以组成最长的回文子序列，即dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);**。

**如果s.charAt(i)与s.charAt(j)相等**，那么**dp[i][j] = dp[i + 1][j - 1] + 2;**

**3、dp数组如何初始化**

首先要考虑当 i 和 j 相同的情况，由递推公式：dp[i][j] = dp[i + 1][j - 1] + 2; 可以看出，递推公式是计算不到 i 和 j 相同的情况的。

因此需要手动初始化以下，当 i 和 j 相等的时候，那么dp[i][j] = 1，即：一个字符的回文子序列的长度就是1。

**4、确定遍历顺序**

由递推公式：dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]); 以及  dp[i][j] = dp[i + 1][j - 1] + 2; 可以看出，**遍历 i 的时候需要从下到上遍历，这样才能保证，下一行的数据都是经过计算过的；遍历 j 的时候需要从左到右遍历，这样也才能保证前一列的数据都是计算过的。**

**5、举例推导dp数组**

输入s:"cbbd" 为例，dp数组状态如图：
![图示](https://img-blog.csdnimg.cn/20210414153141888.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
红色框即：dp[0][s.length() - 1] 为最终结果。

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int n = s.length();
        int[][] dp = new int[n][n];//[i, j]的字符串的最长回文子序列长度是dp[i][j]
        for(int i = n - 1; i >= 0; i--){
            for(int j = i; j < n; j++){
                if(i == j){
                    dp[i][j] = 1;
                    continue;
                }
                if(s.charAt(i) != s.charAt(j)){
                    dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
                }else {
                    dp[i][j] = dp[i + 1][j - 1] + 2;
                }
            }
        }
        return dp[0][n - 1];
    }
}
```