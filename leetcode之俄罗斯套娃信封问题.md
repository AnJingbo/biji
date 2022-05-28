

354. 俄罗斯套娃信封问题
给你一个二维整数数组 envelopes ，其中 envelopes[i] = [wi, hi] ，表示第 i 个信封的宽度和高度。

当另一个信封的宽度和高度都比这个信封大的时候，这个信封就可以放进另一个信封里，如同俄罗斯套娃一样。

请计算 最多能有多少个 信封能组成一组“俄罗斯套娃”信封（即可以把一个信封放到另一个信封里面）。

注意：不允许旋转信封。

示例 1：
输入：envelopes = [[5,4],[6,4],[6,7],[2,3]]
输出：3
解释：最多信封的个数为 3, 组合为: [2,3] => [5,4] => [6,7]。

示例 2：
输入：envelopes = [[1,1],[1,1],[1,1]]
输出：1

提示：
1 <= envelopes.length <= 5000
envelopes[i].length == 2
1 <= wi, hi <= 104

思路：

这道题目其实是最长递增子序列的一个变种，因为很显然，每次合法的嵌套都是大的套小的，相当于找一个最长递增的子序列，其长度就是最多能嵌套的信封个数。

首先按宽度升序排序，当宽度相同的时候按照高度进行升序排序。然后我们利用最长递增子序列的方法，即使用动态规划，定义 dp[i] 表示以 i 结尾的最长递增子序列的长度。对每个 i 的位置，遍历 [0, i)，对两个维度同时判断是否是严格递增的。

```java
class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        Arrays.sort(envelopes, new Comparator<int[]>(){
            public int compare(int[] a, int[] b){
                if(a[0] == b[0]){
                    return a[1] - b[1];
                }
                return a[0] - b[0];
            }
        });
        int[] dp = new int[envelopes.length];
        Arrays.fill(dp, 1);
        int res = 0;
        for(int i = 0; i < envelopes.length; i++){
            for(int j = 0; j < i; j++){
                if(envelopes[i][0] > envelopes[j][0] && envelopes[i][1] > envelopes[j][1]){
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            if(res < dp[i]){
                res = dp[i];
            }
        }
        return res;
    }
}
```