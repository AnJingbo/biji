

494. 目标和
给定一个非负整数数组，a1, a2, ..., an, 和一个目标数，S。现在你有两个符号 + 和 -。对于数组中的任意一个整数，你都可以从 + 或 -中选择一个符号添加在前面。
返回可以使最终数组和为目标数 S 的所有添加符号的方法数。
示例：
输入：nums: [1, 1, 1, 1, 1], S: 3
输出：5
解释：
-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3
一共有5种方法让最终目标和为3。
提示：
数组非空，且长度不会超过 20 。
初始的数组的和不会超过 1000 。
保证返回的最终结果能被 32 位整数存下。

思路：
先让所有的数相加得到总和sum，然后选择一部分数字取负从而使总和调整为目标值S。
本题要如何使表达式的结果为target？
既然为target，那么就一定有 left组合 - right组合 = target。
left + right = sum，而sum是固定的。
那么公式为：left - (sum - left) = target -> left = (target + sum) / 2。
target是固定的，sum是固定的，left就可以求出来。
此时的问题就是在集合nums中找出和为left的组合。

回溯法：
```java
public class Solution {
    int res = 0;
    public int findTargetSumWays(int[] nums, int S) {
        int sum = 0;
        for(int i = 0; i < nums.length; i++){
            sum += nums[i];
        }
        if(S > sum){//此时没有方案
            return 0;
        }
        if((S + sum) % 2 == 1){//判断数组和减去目标和为奇数的情况
            return 0;
        }
        int bagSize = (S + sum) / 2;//转换为组合总和问题，bagSize就是要求的和 
        Arrays.sort(nums);//需要排序
        backtracking(nums, bagSize, 0, 0);
        return res;
    }

    public void backtracking(int[] nums, int target, int sum, int startIndex) {
        if(sum == target) {
            res++;
        }
        for(int i = startIndex; i < nums.length && sum + nums[i] <= target; i++) {
            sum += nums[i];
            backtracking(nums, target, sum, i + 1);
            sum -= nums[i];
        }
    }
}
```
```java
public class Solution {
    int sum = 0;
    int res = 0;
    public int findTargetSumWays(int[] nums, int S) {
        backtracking(nums, 0, S);
        return res;
    }

    public void backtracking(int[] nums, int i, int S) {
        if(i == nums.length){
            if(sum == S){
                res++;
            }
            return;
        }
        sum += nums[i];
        backtracking(nums, i + 1, S);
        sum -= nums[i];

        sum -= nums[i];
        backtracking(nums, i + 1, S);
        sum += nums[i];
    }
}
```
动态规划：
如何转换为 01背包问题呢？
先让所有的数相加得到总和sum，然后选择一部分数字取负从而使总和调整为目标值S。
假设加法的总和为 x，那么减法对应的总和就是 sum - x。
所以我们要求的是 x - (sum - x) = S。
x = (S + sum) / 2。
即加法的总和应该为(S + sum) / 2。
此时问题就转化为：装满背包容量为 x 的背包，有几种方法。

当看到(S + sum) / 2 应该担心计算的过程中向下取整有没有影响。

这么担心就对了，例如sum 是5，S是2的话其实就是无解的，所以：
```java
if ((S + sum) % 2 == 1) return 0; // 此时没有方案
```
看到这种表达式，应该本能的反应，两个int相加数值可能溢出的问题，当然本题并没有溢出。

再回归到01背包问题，为什么是01背包呢？

因为每个物品（题目中的1）只用一次！

这次和之前遇到的背包问题不一样了，之前都是求容量为j的背包，最多能装多少。

本题则是装满有几种方法。其实这就是一个组合问题了。
**1、确定dp数组以及下标含义**
dp[j] 表示：填满容量为 j（包括j）的包，有dp[i]种方法。
其实也可以使用二维dp数组来求解本题，dp[i][j]：使用 下标为 0 到 i 的nums[i]能够凑满j（包括j）这么大容量的包，有dp[i][j]种方法。
**2、确定递推公式**
有哪些来源可以推出dp[j]呢？
不考虑nums[i]的情况下，填满容量为j - nums[i]的背包，有dp[j - nums[i]]种方法。
那么只要搞到nums[i]的话，凑成dp[j]就有dp[j - nums[i]] 种方法。
举一个例子,nums[i] = 2：dp[3]，填满背包容量为3的话，有dp[3]种方法。
那么只需要搞到一个2（nums[i]），有dp[3]方法可以凑齐容量为3的背包，相应的就有多少种方法可以凑齐容量为5的背包。
那么需要把 这些方法累加起来就可以了，dp[i] += dp[j - nums[j]]
所以求组合类问题的公式，都是类似这种：
```java
dp[j] += dp[j - nums[i]]
```
这个公式在后面在讲解背包解决排列组合问题的时候还会用到！
**这道题的关键不是nums[i]的选与不选，而是nums[i]是加还是减**，那么我们就可以将方程定义为：

```java
dp[i][j] = dp[i - 1][j - nums[i]] + dp[i - 1][j + nums[i]]
```

**3、dp数组如何初始化**
从递归公式可以看出，在初始化的时候dp[0] 一定要初始化为1，因为dp[0]是在公式中一切递推结果的起源，如果dp[0]是0的话，递归结果将都是0。

dp[0] = 1，理论上也很好解释，装满容量为0的背包，有1种方法，就是装0件物品。

dp[j]其他下标对应的数值应该初始化为0，从递归公式也可以看出，dp[j]要保证是0的初始值，才能正确的由dp[j - nums[i]]推导出来。
**4、确定遍历顺序**
一维dp的遍历，nums放在外循环，target在内循环，且内循环倒序。
**5、举例推导dp数组**
输入：nums: [1, 1, 1, 1, 1], S: 3

bagSize = (S + sum) / 2 =   (3 + 5) / 2 = 4

dp数组状态变化如下：

![图示](https://img-blog.csdnimg.cn/20210125172901970.png)
注：
在求装满背包有几种方法的情况下，递推公式一般为：
```java
dp[j] += dp[j - nums[i]];
```

```java
public class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        int sum = 0;
        for(int i = 0; i < nums.length; i++){
            sum += nums[i];
        }
        if((S + sum) % 2 == 1 || sum < S){
            return 0;
        }
        int bagSize = (S + sum) / 2; 
        int[] dp = new int[bagSize + 1];
        dp[0] = 1;
        for(int i = 0; i < nums.length; i++){
            for(int j = bagSize; j >= nums[i]; j--){
                dp[j] += dp[j - nums[i]];
            }
        }
        return dp[bagSize];
    }
}
```

另：

这个问题可以转化为一个子集划分问题，而子集划分问题又是一个典型的背包问题。

首先，如果我们把 nums 划分成两个子集 A 和 B，分别代表分配 + 的数和分配 - 的数，那么他们和 target 存在如下关系：
```java
sum(A) - sum(B) = target
sum(B) = sum(nums) - sum(A)
由上面两个式子推出：
sum(A) - (sum(nums) - sum(A)) = target
2 * sum(A) = target + sum(nums)
```

综上，可以推出 sum(A) = (target + sum(nums)) / 2，也就是把原问题转化成：**nums 中存在几个子集 A，使得 A 中元素的和为 (target + sum(nums)) / 2**

好的，变成背包问题的标准形式：

**有一个背包，容量为 sum，现在给你 N 个物品，第 i 个物品的重量为 nums[i - 1]（注意 1 <= i <= N），每个物品只有一个，请问你有几种不同的方法能够恰好装满这个背包？**

现在，这就是一个正宗的动态规划问题了，下面按照我们一直强调的动态规划套路走流程：

**第一步要明确两点，「状态」和「选择」。**

对于背包问题，这个都是一样的，状态就是「背包的容量」和「可选择的物品」，选择就是「装进背包」或者「不装进背包」。

**第二步要明确 dp 数组的定义。**

按照背包问题的套路，可以给出如下定义：

**dp[i][j] = x 表示，若只在前 i 个物品中选择，若当前背包的容量为 j，则最多有 x 种方法可以恰好装满背包。**

翻译成我们探讨的子集问题就是，若只在 nums 的前 i 个元素中选择，若目标和为 j，则最多有 x 种方法划分子集。

根据这个定义，显然 **dp[0][..] = 0，因为没有物品的话，根本没办法装背包；dp[..][0] = 1，因为如果背包的最大载重为 0，「什么都不装」就是唯一的一种装法。**

我们所求的答案就是 dp[N][sum]，即使用所有 N 个物品，有几种方法可以装满容量为 sum 的背包。

第三步，根据「选择」，思考状态转移的逻辑。

回想刚才的 dp 数组含义，可以根据「选择」对 dp[i][j] 得到以下状态转移：

**如果不把 nums[i] 算入子集**，或者说你不把这第 i 个物品装入背包，那么恰好装满背包的方法数就取决于上一个状态 dp[i-1][j]，继承之前的结果。

**如果把 nums[i] 算入子集**，或者说你把这第 i 个物品装入了背包，那么只要看前 i - 1 个物品有几种方法可以装满 j - nums[i-1] 的重量就行了，所以取决于状态 dp[i-1][j-nums[i-1]]。

PS：**注意我们说的 i 是从 1 开始算的，而数组 nums 的索引时从 0 开始算的，所以 nums[i-1] 代表的是第 i 个物品的重量，j - nums[i-1] 就是背包装入物品 i 之后还剩下的容量。**

由于 dp[i][j] 为装满背包的总方法数，所以应该以上两种选择的结果求和，得到状态转移方程：

dp[i][j] = dp[i-1][j] + dp[i-1][j-nums[i-1]];

然后，发现这个 dp[i][j] 只和前一行 dp[i-1][..] 有关，那么肯定可以优化成一维 dp。

对照二维 dp，只要把 dp 数组的第一个维度全都去掉就行了，唯一的区别就是这里的 j 要从后往前遍历，原因如下：

因为**二维压缩到一维的根本原理是，dp[j] 和 dp[j-nums[i-1]] 还没被新结果覆盖的时候，相当于二维 dp 中的 dp[i-1][j] 和 dp[i-1][j-nums[i-1]]**。

那么，我们就要做到：在计算新的 dp[j] 的时候，dp[j] 和 dp[j-nums[i-1]] 还是上一轮外层 for 循环的结果。

**如果你从前往后遍历一维 dp 数组，dp[j] 显然是没问题的，但是 dp[j-nums[i-1]] 已经不是上一轮外层 for 循环的结果了**，这里就会使用错误的状态，当然得不到正确的答案。

现在，这道题算是彻底解决了。
```java
public class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        int sum = 0;
        for(int i = 0; i < nums.length; i++){
            sum += nums[i];
        }
        if((S + sum) % 2 == 1 || sum < S){
            return 0;
        }
        int target = (S + sum) / 2;
        int[][] dp = new int[nums.length + 1][target + 1];//前 i 个元素任意取，填满容量为 j 的背包的方法数有 dp[i][j] 个
        for(int i = 0; i <= nums.length; i++){
            dp[i][0] = 1;
        }
        for(int i = 1; i <= nums.length; i++){
            for(int j = 0; j <= target; j++){//这里必须从0开始，不能从1开始
                if(j < nums[i - 1]){
                    dp[i][j] = dp[i - 1][j];
                }else{
                    dp[i][j] = dp[i - 1][j] + dp[i - 1][j - nums[i - 1]];
                }
            }
        }
        return dp[nums.length][target];
    }
}
```