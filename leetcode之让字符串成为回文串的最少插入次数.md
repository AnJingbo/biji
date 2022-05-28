

1312. 让字符串成为回文串的最少插入次数

给你一个字符串 s ，每一次操作你都可以在字符串的任意位置插入任意字符。
请你返回让 s 成为回文串的 最少操作次数 。
「回文串」是正读和反读都相同的字符串。

示例：
输入：s = "leetcode"
输出：5
解释：插入 5 个字符后字符串变为 "leetcodocteel" 。

**1、确定 dp 数组及其下标的含义**

dp[i][j]：对于字符串 s[i ... j]，最少需要进行 dp[i][j] 次插入才能变成回文串。

注：dp数组的定义暗含了 j 的值是大于等于 i 的值的。

**2、确定递推公式**

如果 s.charAt(i) == s.charAt(j)，那么 dp[i][j] 就等于 dp[i - 1][j + 1]。因为已经算出 dp[i - 1][j + 1]，即知道了 s[i+1 ... j-1] 成为回文串的最小插入次数，那么也就可以认为 s[i+1 ... j-1] 已经是一个回文串了，因此如果 s.charAt(i) == s.charAt(j)，那么就不再需要任何插入。即 dp[i][j] = dp[i - 1][j + 1]。

如果 s.charAt(i) != s.charAt(j)，那么 dp[i][j] = Math.min(dp[i + 1][j], dp[i][j - 1]) + 1; 即：需要选择把 s[i+1 ... j] 或者 s[i ... j-1]变成回文串的最小插入次数，再加 1（这个加 1 就是在右边上插入一个s[i] 或者 在左边插入一个s[j]）。注意不能无脑选：dp[i - 1][j + 1] + 2 这种情况，比如：ab 字符串，其实插入一次就能变成回文串了，但如果选择 dp[i - 1][j + 1] + 2 则需要插入 2 次才变成字符串。

**3、如何初始化**

当 i == j 的时候，此时 s[i ... j] 只有一个字符，此时就不需要插入，因此就是 0。

```java
class Solution {
    public int minInsertions(String s) {
        int n = s.length();
        int[][] dp = new int[n][n];// 对于字符串 s[i ... j]，最少需要进行 dp[i][j] 次插入才能变成回文串
        for(int i = n - 1; i >= 0; i--){
            for(int j = i; j < n; j++){
                if(i == j){
                    continue;
                }
                if(s.charAt(i) == s.charAt(j)){
                    dp[i][j] = dp[i + 1][j - 1];
                }else{
                    dp[i][j] = Math.min(dp[i + 1][j], dp[i][j - 1]) + 1;
                }
            }
        }
        return dp[0][n - 1];
    }
}
```