

![图示](https://img-blog.csdnimg.cn/163aa59386084da481c21eb984395dca.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
* 注意：保证每次出现字符 * 时，前面都匹配到有效的字符

```java
class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length(), n = p.length();
        boolean[][] dp = new boolean[m + 1][n + 1]; // s 的前 i 个字符和 p 的前 j 个字符是否匹配
        // 如果 s 和 p 都为空，则肯定匹配
        dp[0][0] = true;
        // 如果 s 不为空，p 为空，则肯定匹配不成功
        // 如果 s 为空，p 不为空
        for(int j = 2; j <= n; j++){
            // 此时比如：c*a*b* 能够和 "" 匹配。即 p 的偶数位为 * 时才能够匹配
            if(p.charAt(j - 1) == '*'){
                dp[0][j] = dp[0][j - 2];
            }
        }

        for(int i = 1; i <= m; i++){
            for(int j = 1; j <= n; j++){
                // 如果 s 的第 i 个元素和 p 的第 j 个元素相等，或者 p 的第 j 个元素是 .
                if(s.charAt(i - 1) == p.charAt(j - 1) || p.charAt(j - 1) == '.'){
                    dp[i][j] = dp[i - 1][j - 1];
                }
                // 如果 p 的第 j 个元素是 *
                if(p.charAt(j - 1) == '*'){
                    // 如果 s 的第 i 个元素和 p 的第 j-1 个元素相等，或者 p 的第 j-1 个元素是 .
                    if(s.charAt(i - 1) == p.charAt(j - 2) || p.charAt(j - 2) == '.'){
                        dp[i][j] = dp[i][j - 2] || dp[i - 1][j];
                    }else{
                        dp[i][j] = dp[i][j - 2];
                    }
                }
            }
        }
        return dp[m][n];
    }
}
```