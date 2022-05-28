

![题目](https://img-blog.csdnimg.cn/20210114144037514.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
思路：
![示例1](https://img-blog.csdnimg.cn/20210114151106959.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
![示例2](https://img-blog.csdnimg.cn/20210114151044108.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
当n为3的时候，有以下几种情况：
1、当1是头结点的时候，其右子树有两个结点，这两个结点的布局，其实和 n 为2的时候两棵树的布局是一样的。
（可能有同学问了，这布局不一样啊，节点数值都不一样。别忘了我们就是求不同树的数量，并不用把搜索树都列出来，所以不用关心其具体数值的差异）
2、当2是头结点的时候，其左右子树都只有一个结点，其布局和n为1的时候只有一棵树的布局也是一样的。
3、当3是头结点的时候，其左子树有两个结点，这两个结点的布局和n为2的时候两棵树的布局也是一样的。

发现到这里，其实我们就找到了重叠子问题了，其实也就是发现可以通过dp[1] 和 dp[2] 来推导出来dp[3]的某种方式。

dp[3]，就是 元素1为头结点搜时索树的数量 + 元素2为头结点时搜索树的数量 + 元素3为头结点时搜索树的数量，
元素1为头结点时搜索树的数量 = 右子树有2个元素的搜索树的数量 * 左子树有0个元素的搜索树的数量；
元素2为头结点时搜索树的数量 = 右子树有1个元素的搜索树的数量 * 左子树有1个元素的搜索树的数量；
元素3为头结点时搜索树的数量 = 右子树有0个元素的搜索树的数量 * 左子树有2个元素的搜索树的数量。
有2个元素的搜索树数量就是dp[2]。
有1个元素的搜索树数量就是dp[1]。
有0个元素的搜索树数量就是dp[0]。

所以dp[3] = dp[2] * dp[0] + dp[1] * dp[1] + dp[0] * dp[2]
动态规划五部曲：
**1、确定dp数组以及下标的含义**
dp[i]：有 i 个结点时二叉搜索树的种类。
**2、确定递推公式**
在上面分析中，其实已经得出其递推关系，dp[i] += dp[以 j 为头结点时左子树结点个数] * dp[以 j 为头结点时右子树结点个数]。j 相当于时头结点的元素，从1遍历到 i 为止。
所以递推公式为：dp[i] += dp[j - 1] * dp[i - j]。j - 1：j 为头结点的时候左子树结点的个数；i - j：j为头结点的时候右子树结点的个数。
**3、dp数组如何初始化**
初始化时，只需要初始化dp[0]即可。
从定义上来讲，空结点也是一棵二叉树，也是一棵二叉搜索树，这是可以说得通的。
从递推公式上来讲，dp[以 j 为头结点时左子树结点个数] * dp[以 j 为头结点时右子树结点个数]中即使以 j 为头结点时左子树结点个数为0，也需要dp[以 j 为头结点时左子树结点个数] = 1，否则乘法的结果就都变成0了。
所以初始化dp[0] = 1。
**4、确定遍历顺序**
由递推公式：dp[i] += dp[j - 1] * dp[i - j]可以看出，结点数为 i 的状态是依靠 i 之前结点数的状态。
那么遍历 i 里面每一个数作为头结点的状态，通过 j 来遍历。
**5、举例推导dp数组**
n为5时dp数组状态如图：
![图示](https://img-blog.csdnimg.cn/20210114150515672.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n + 1];// 一共 i 个结点时，可以组成的二叉搜索树的种类
        dp[0] = 1;
        for(int i = 1; i <= n; i++){
            for(int j = 1; j <= i; j++){// 一共 i 个结点，以 j 结点为根时的二叉搜索树的种类
                dp[i] += dp[j - 1] * dp[i - j];// 比 j 结点小的结点个数有 j-1 个，比 j 结点大的结点个数有 i-j 个     
            }
        }
        return dp[n];
    }
}
```
自己写的：
```java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n + 1];//结点数为 i 的时候能组成的二叉搜索树的种类为dp[i]
        dp[1] = 1;
        for(int i = 2; i <= n; i++){
            int numOfLeft = 1;
            int numOfRight = i - 2;
            for(int j = 1; j <= i; j++){
                if(j == 1 || j == i){
                    dp[i] += dp[i - 1];
                }else{
                    dp[i] += dp[numOfLeft++] * dp[numOfRight--];
                }
            }
        }
        return dp[n];
    }
}
```