

474. 一和零
给你一个二进制字符串数组 strs 和两个整数 m 和 n 。
请你找出并返回 strs 的最大子集的大小，该子集中 最多 有 m 个 0 和 n 个 1 。
如果 x 的所有元素也是 y 的元素，集合 x 是集合 y 的 子集 。
示例 1：
输入：strs = ["10", "0001", "111001", "1", "0"], m = 5, n = 3
输出：4
解释：最多有 5 个 0 和 3 个 1 的最大子集是 {"10","0001","1","0"} ，因此答案是 4 。
其他满足题意但较小的子集包括 {"0001","1"} 和 {"10","1","0"} 。{"111001"} 不满足题意，因为它含 4 个 1 ，大于 n 的值 3 。
示例 2：
输入：strs = ["10", "0", "1"], m = 1, n = 1
输出：2
解释：最大的子集是 {"0", "1"} ，所以答案是 2 。
提示：
1 <= strs.length <= 600
1 <= strs[i].length <= 100
strs[i] 仅由 '0' 和 '1' 组成
1 <= m, n <= 100


动规五部曲：
**1、确定dp数组（dp table）以及下标的含义**
dp[i][j]：最多有 i 个0和 j 个1的strs的最大子集的大小为dp[i][j]。

**2、确定递推公式**
dp[i][j] 可以由前一个strs里的字符串推导出来，strs里的字符串有zeroNum个0，oneNum个1。

dp[i][j] 就可以是 dp[i - zeroNum][j - oneNum] + 1。

然后我们在遍历的过程中，取dp[i][j]的最大值。

所以递推公式：dp[i][j] = max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);

此时大家可以回想一下01背包的递推公式：dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);

对比一下就会发现，字符串的zeroNum和oneNum相当于物品的重量（weight[i]），字符串的个数相当于物品的价值（value[i]），每个字符串的价值相当于是1。

这就是一个典型的01背包！ 只不过物品的重量有了两个维度而已。

**3、dp数组如何初始化**
因为物品价值不会是负数，初始为0，保证递推的时候dp[i][j]不会被初始值覆盖。

**4、确定遍历顺序**
一维01背包一定是外层for循环遍历物品，内层for循环遍历背包容量且从后向前遍历！

那么本题也是，物品就是strs里的字符串，背包容量就是题目描述中的m和n。

代码如下：

```java
for(String str : strs){//遍历物品
    int zeroNum= 0;
    int oneNum= 0;
    for(int i = 0; i < str.length(); i++){
        if(str.charAt(i) == '0'){
            zeroNum++;
        }else{
            oneNum++;
        }
    }
    for(int i = m; i >= zeroNum; i--){//遍历背包容量且从后向前遍历
        for(int j = n; j >= oneNum; j--){
            dp[i][j] = Math.max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);
        }
    }
}
```

有同学可能想，那个遍历背包容量的两层for循环先后循序有没有什么讲究？

没讲究，都是物品重量的一个维度，先遍历那个都行！

**5、举例推导dp数组**
以输入：["10","0001","111001","1","0"]，m = 3，n = 3为例

最后dp数组的状态如下所示：
![图示](https://img-blog.csdnimg.cn/20210126164534340.png)

```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        int[][] dp = new int[m + 1][n + 1];
        for(String str : strs){//遍历物品
            int zeroNum= 0;
            int oneNum= 0;
            for(int i = 0; i < str.length(); i++){
                if(str.charAt(i) == '0'){
                    zeroNum++;
                }else{
                    oneNum++;
                }
            }
            for(int i = m; i >= zeroNum; i--){//遍历背包容量且从后向前遍历
                for(int j = n; j >= oneNum; j--){
                    dp[i][j] = Math.max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);
                }
            }
        }
        return dp[m][n];
    }
}
```