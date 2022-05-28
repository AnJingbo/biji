

647. 回文子串
给定一个字符串，你的任务是计算这个字符串中有多少个回文子串。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。

示例 1：
输入："abc"
输出：3
解释：三个回文子串: "a", "b", "c"

示例 2：
输入："aaa"
输出：6
解释：6个回文子串: "a", "a", "a", "aa", "aa", "aaa"

提示：输入的字符串长度不会超过 1000 。

思路：

**1、确定dp数组（dp table）以及下标的含义**

布尔类型的dp[i][j]：**区间范围[i, j]的子串是否是回文串，如果是则dp[i][j]为true，否则为false。注意：因为是区间，所以本题隐含了的条件是：j>=i，因此下面的第二层循环中的 j 是从 i 开始的。**

**2、确定递推公式**

**如果s.charAt(i)与s.charAt(j)不相等，那么dp[i][j]一定是false。**

**如果s.charAt(i)与s.charAt(j)相等，那么有如下三种情况：
情况一：下标 i 与 j 相同，同一个字符例如 a，当然是回文子串
情况二：下标 i 与 j 相差为1，例如aa，也是回文子串
情况三：下标 i 与 j 相差大于1的时候，例如abcba，此时s.charAt(i)与s.charAt(j)已经相同了，我们看 i 到 j 区间是不是回文子串就看bcb是不是回文就可以了，那么bcb的区间就是 [i+1, j-1]区间，这个区间是不是回文就看dp[i + 1][j - 1]是否为true。**

**3、dp数组如何初始化**

dp[i][j]可以初始化为true么？当然不行，怎能刚开始就全都匹配上了。

所以dp[i][j]初始化为false。

**4、确定遍历顺序**

遍历顺序可有有点讲究了。

首先从递推公式中可以看出，情况三是根据dp[i + 1][j - 1]是否为true，再对dp[i][j]进行赋值的。

dp[i + 1][j - 1] 在 dp[i][j]的左下角，如图：
![图示](https://img-blog.csdnimg.cn/20210412223233862.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
如果这矩阵是从上到下，从左到右遍历，那么会用到没有计算过的dp[i + 1][j - 1]，也就是根据不确定是不是回文的区间[i+1,j-1]，来判断了[i, j]是不是回文，那结果一定是不对的。

**所以一定要从下到上，从左到右遍历，这样保证dp[i + 1][j - 1]都是经过计算的。**

有的代码实现是优先遍历列，然后遍历行，其实也是一个道理，都是为了保证dp[i + 1][j - 1]都是经过计算的。

**5、举例推导dp数组**

举例，输入："aaa"，dp[i][j]状态如下：
![图示](https://img-blog.csdnimg.cn/20210412223518446.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
class Solution {
    public int countSubstrings(String s) {
        int n = s.length();
        boolean[][] dp = new boolean[n][n];// 区间范围[i, j]的子串是否是回文串，如果是则dp[i][j]为true，否则为false
        int res = 0;
        for(int i = n - 1; i >= 0; i--){
            for(int j = i; j < n; j++){
                if(s.charAt(i) != s.charAt(j)){
                    dp[i][j] = false;
                }else{
                    if(j - i <= 1){
                        dp[i][j] = true;
                        res++;
                    }else if(dp[i + 1][j - 1]){
                        dp[i][j] = true;
                        res++;
                    }
                }
            }
        }
        return res;
    }
}
```

```java
class Solution {
    public int countSubstrings(String s) {
        int res = 0;
        for(int i = 0; i < s.length(); i++){
            int num1 = count(s, i, i);
            int num2 = count(s, i, i + 1);
            res += num1 + num2;
        }
        return res;
    }
    public int count(String s, int l, int r){
        int res = 0;
        //防止索引越界
        while(l >= 0 && r < s.length() && s.charAt(r) == s.charAt(l)){
            res++;
            //向两边扩散
            l--;
            r++;
        }
        return res;
    }
}
```