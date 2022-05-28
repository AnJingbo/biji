

62. 不同路径
一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。
机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。
问总共有多少条不同的路径？
例：
![例题](https://img-blog.csdnimg.cn/20210111165852309.png)
输入：m = 3, n = 7
输出：28

思路：
方法一：动态规划
**1、确定dp数组以及下标的含义**
dp[i][j]：表示从(0, 0)出发，到(i, j)有dp[i][j]条不同的路径。
**2、确定递推公式**
要想求dp[i][j]，只能有两个方向来推导出来，即dp[i - 1][j]和dp[i][j - 1]。
此时再回顾一下，dp[i - 1][j]是表示啥，是从(0, 0)的位置到(i - 1, j)有几条路径，dp[i][j - 1]同理。
所以dp[i][j] = dp[i - 1][j] - dp[i][j - 1]，因为dp[i][j]只有之两个方向 过来。
**3、dp数组的初始化**
dp[i][0]一定都是1，因为从(0, 0)的位置到(i, 0)的路径只有一条，那么dp[0][j]也同理。
**4、确定递归顺序**
由dp[i][j] = dp[i - 1][j] - dp[i][j - 1]，dp[i][j]都是从其上方和左方推导而来，那么从左到右一层一层遍历即可。
这样就可以保证推导dp[i][j]的时候，dp[i - 1][j]和dp[i][j - 1]一定都是有数值的。
**5、举例推导dp数组**
![图示](https://img-blog.csdnimg.cn/20210111171337298.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        for(int j = 0; j < n; j++){
            dp[0][j] = 1;
        }
        for(int i = 0; i < m; i++){
            dp[i][0] = 1;
        }
        for(int i = 1; i < m; i++){
            for(int j = 1; j < n; j++){
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
}
```
自己写的：只初始化了dp[0][0]为1
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        dp[0][0] = 1;
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(i >= 1){
                    dp[i][j] += dp[i - 1][j];
                }
                if(j >= 1){
                    dp[i][j] += dp[i][j - 1];
                }
            }
        }
        return dp[m - 1][n - 1];
    }
}
```
滚动数组写法：
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[] dp = new int[n];
        for(int i = 0; i < n; i++){
            dp[i] = 1;
        }
        for(int i = 1; i < m; i++){
            for(int j = 1; j < n; j++){
                dp[j] += dp[j - 1];
            }
        }
        return dp[n - 1];
    }
}
```

方法二：深搜(会超时)
```java
class Solution {
    int res = 0;
    public int uniquePaths(int m, int n) {
        dfs(1, 1, m, n);
        return res;
    }
    public void dfs(int i, int j, int m, int n){
        if(i == m && j == n){
            res++;
            return;
        }
        if(i < m){
            dfs(i + 1, j, m, n);
        }
        if(j < n){
            dfs(i, j + 1, m, n);
        }    
    }
}
```
优化一下：
```java
class Solution {
    public int uniquePaths(int m, int n) {
        return dfs(1, 1, m, n);
    }
    public int dfs(int i, int j, int m, int n){
        if(i > m || j > n){
            return 0;
        }
        if(i == m && j == n){
            return 1;
        }
        return dfs(i + 1, j, m, n) + dfs(i, j + 1, m, n);
    }
}
```