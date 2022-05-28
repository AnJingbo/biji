

72. 编辑距离
给你两个单词 word1 和 word2，请你计算出将 word1 转换成 word2 所使用的最少操作数 。

你可以对一个单词进行如下三种操作：
插入一个字符
删除一个字符
替换一个字符

示例 1：
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
示例 2：
输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')

提示：
0 <= word1.length, word2.length <= 500
word1 和 word2 由小写英文字母组成

思路：
base case 是i走完s1或j走完s2，可以直接返回另一个字符串剩下的长度

对于每对儿字符s1.charAt(i)和s2.charAt(j)，可以有四种操作：

```java
if s1.charAt(i) == s2.charAt(j):
    啥都别做（skip）
    i, j 同时向前移动
else:
    三选一：
        插入（insert）
        删除（delete）
        替换（replace）
```

有这个框架，问题就已经解决了。那么「三选一」到底该怎么选择呢？很简单，全试一遍，哪个操作最后得到的编辑距离最小，就选谁。这里需要递归技巧，理解需要点技巧，先看下代码：
```java
class Solution {
    String s1;
    String s2;
    public int minDistance(String word1, String word2) {
        s1 = word1;
        s2 = word2;
        return dp(s1.length() - 1, s2.length() - 1);//i, j初始化指向最后一个索引
    }
    public int dp(int i, int j){
        //base case
        if(i == -1){//如果i走完s1时j还没走完s2，那就只能用插入操作把s2剩下的字符全部插入s1
            return j + 1;
        }
        if(j == -1){//如果j走完s2时i还没走完s1，那么只能用删除操作把s1缩短为s2
            return i + 1;
        }
        if(s1.charAt(i) == s2.charAt(j)){
            return dp(i - 1, j - 1);//啥都不做
        }else{
            return getMin(
                dp(i, j - 1) + 1, // 插入
                dp(i - 1, j) + 1, // 删除
                dp(i - 1, j - 1) + 1 // 替换
            );
        }  
    }
    public int getMin(int a, int b, int c){
        int min = Math.min(a, b);
        return min <= c ? min : c;
    }
}
```
解释一下递归部分：
首先，这里 dp(i, j) 函数的定义是这样的：**返回 s1[0..i] 和 s2[0..j] 的最小编辑距离。**
明确定义后：

```java
if s1.charAt(i) == s2.charAt(j):
    return dp(i - 1, j - 1)// 啥都不做
解释：
本来就相等，不需要任何操作
s1[0..i] 和 s2[0..j] 的最小编辑距离等于
s1[0..i-1] 和 s2[0..j-1] 的最小编辑距离
也就是说 dp(i, j) 等于 dp(i-1, j-1)
```

**如果s1.charAt(i) != s1.charAt(j)，就要对三个操作递归了**，稍微需要点思考：
```java
dp(i, j - 1) + 1, // 插入
解释：
我直接在 s1[i] 插入一个和 s2[j] 一样的字符
那么 s2[j] 就被匹配了，前移 j，继续跟 i 对比
别忘了操作数加一
```
```java
dp(i - 1, j) + 1, // 删除
解释：
我直接把 s[i] 这个字符删掉
前移 i，继续跟 j 对比
操作数加一
```

```java
dp(i - 1, j - 1) + 1 // 替换
解释：
我直接把 s1[i] 替换成 s2[j]，这样它俩就匹配了
同时前移 i，j 继续对比
操作数加一
```
动规优化：

dp[i][j]：**存储 s1[0..i-1] 和 s2[0..j-1] 的最小编辑距离**

或者：dp[i][j] 表示**以下标i-1为结尾的字符串word1，和以下标j-1为结尾的字符串word2，最近编辑距离为dp[i][j]。**

 dp 数组和递归 dp 函数含义一样，也就可以直接套用之前的思路写代码，唯一不同的是，**DP table 是自底向上求解，递归解法是自顶向下求解**：
```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length(), n = word2.length();
        int[][] dp = new int[m + 1][n + 1]; // word1 的前 i 个字符变成 word2 的前 j 个字符所需的最少操作次数是 dp[i][j]
        // 需要考虑 word1 或 word2 一个字母都没有，即全增加/删除的情况
        for(int i = 0; i <= m; i++){
            dp[i][0] = i;
        }
        for(int j = 0; j <= n; j++){
            dp[0][j] = j;
        }

        for(int i = 1; i <= m; i++){
            for(int j = 1; j <= n; j++){
                if(word1.charAt(i - 1) == word2.charAt(j - 1)){//第i个字符对应的下标是i-1
                    dp[i][j] = dp[i - 1][j - 1];
                }else{
                    dp[i][j] = getMin(dp[i][j - 1] + 1, dp[i - 1][j] + 1, dp[i - 1][j - 1] + 1);//插入，删除，替换
                }
            }
        }
        return dp[m][n];
    }
    
    public int getMin(int a, int b, int c){
        int min = Math.min(a, b);
        return min <= c ? min : c;
    }
}
```
只求出了最小的编辑距离，那么具体操作是什么？
这个其实很简单，只需要给dp数组添加额外的信息即可：
```java
// int[][] dp;
Node[][] dp;

class Node {
    int val;
    int choice;
    // 0 代表啥都不做
    // 1 代表插入
    // 2 代表删除
    // 3 代表替换
}
```
val属性就是之前的 dp 数组的数值，choice属性代表操作。在做最优选择时，顺便把操作记录下来，然后就从结果反推具体操作。