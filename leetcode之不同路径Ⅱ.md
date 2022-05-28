

63. 不同路径 II
一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。
机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。
现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？
![题目](https://img-blog.csdnimg.cn/20210112144139893.png)
网格中的障碍物和空位置分别用 1 和 0 来表示。
示例：
![图示](https://img-blog.csdnimg.cn/20210112144215419.jpg)
输入：obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
输出：2
解释：
3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 2 条不同的路径：
64. 向右 -> 向右 -> 向下 -> 向下
65. 向下 -> 向下 -> 向右 -> 向右
思路：
动规五部曲：
**1、确定dp数组以及下标的含义**
dp[i][j]：表示从(0, 0)出发，到(i, j)有dp[i][j]条不同的路径。
**2、确定递推公式**
dp[i][j] = dp[i - 1][j] - dp[i][j - 1]。
但这里需要注意：因为有了障碍，(i, j)如果就是障碍的话应该保持初始状态（初始状态为0）。
**3、dp数组如何初始化**
从(0, 0)的位置到(i, 0)的路径只有一条，所以dp[i][0]一定为1，dp[0][j]也同理。但如果(i, 0) 这条边有了障碍之后，障碍之后（包括障碍）都是走不到的位置了，所以障碍之后的dp[i][0]应该还是初始值0。下标(0, j)的初始化情况同理。
如图：
![图示1](https://img-blog.csdnimg.cn/20210112145045212.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
**4、确定遍历顺序**
从递归公式dp[i][j] =  dp[i - 1][j] + dp[i][j - 1] 中可以看出，一定是从左到右一层一层遍历，这样保证推导dp[i][j]的时候，dp[i - 1][j] 和 dp[i][j - 1]一定是有数值。
**5、举例推导dp数组**
示例对应的dp数组：
![图示2](https://img-blog.csdnimg.cn/20210112145320392.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        int[][] dp = new int[m][n];
        for(int j = 0; j < n && obstacleGrid[0][j] == 0; j++){
            dp[0][j] = 1;
        }
        for(int i = 0; i < m && obstacleGrid[i][0] == 0; i++){
            dp[i][0] = 1;
        }
        for(int i = 1; i < m; i++){
            for(int j = 1; j < n; j++){
                if(obstacleGrid[i][j] == 1){
                    continue;
                }
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
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        int[][] dp = new int[m][n];
        dp[0][0] = 1;
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(obstacleGrid[i][j] == 1){
                    dp[i][j] = 0;
                }else{
                    if(i - 1 >= 0){
                        dp[i][j] += dp[i - 1][j];
                    }
                    if(j - 1 >= 0){
                        dp[i][j] += dp[i][j - 1];
                    }
                }
            }
        }
        return dp[m - 1][n - 1];
    }
}
```