

### 486. 预测赢家
![ts](https://img-blog.csdnimg.cn/f6fd1a7506e14b67945e638b1a764f14.png)
```java
class Solution {
    public boolean PredictTheWinner(int[] nums) {
        // 剩余 [left,right] 个石子堆时，在双方都做最好选择的情况下，先手比后手多的最大石子个数（即先手与后手的最大得分差值）
        int[][] dp = new int[nums.length][nums.length];
        /** 比如：[5,3,2,4]
        假设当 i=0,j=3 时，Alice有两种选择：
        (1)一种是选择最左边的5，然后由于下一轮对手是先手，因此对手可以在区间[1,3]中做最优决策，对手能赢我的最大差值为dp[1][3]，但是该分数对我来说是负数，因此dp[i][j]=nums[0]-dp[1][3]
        (2)一种是选择最右边的4，然后由于下一轮对手是先手，因此对手可以在区间[0,2]中做最优决策，对手能赢我的最大差值为dp[0][2]，但是该分数对我来说是负数，因此dp[i][j]=nums[3]-dp[0][2]
        因此状态转移方程就是 dp[i][j]=Math.max(piles[i]-dp[i+1][j], piles[j]-dp[i][j-1])
         */
        for(int i = 0; i < nums.length; i++){
            dp[i][i] = nums[i];
        }
        for(int i = nums.length - 1; i >= 0; i--){
            for(int j = i + 1; j < nums.length; j++){
                dp[i][j] = Math.max(nums[i] - dp[i + 1][j],
                                    nums[j] - dp[i][j - 1]);
            }
        }
        return dp[0][nums.length - 1] >= 0;
    }
}
```
```java
class Solution {
    int[][] memo;
    public boolean PredictTheWinner(int[] nums) {
        int n = nums.length;
        memo = new int[n][n];
        for(int i = 0; i < n; i++){
            Arrays.fill(memo[i], Integer.MIN_VALUE);
        }
        return getScore(nums, 0, n - 1) >= 0;
    }
    // 面对[left ... right]个石子堆，当前玩家可以赢得对方的最大分数
    public int getScore(int[] nums, int left, int right){
        if(left == right){
            return nums[left];
        }
        if(memo[left][right] != Integer.MIN_VALUE){
            return memo[left][right];
        }
        int leftScore = nums[left] - getScore(nums, left + 1, right);
        int rightScore = nums[right] - getScore(nums, left, right - 1);
        memo[left][right] = Math.max(leftScore, rightScore);
        return memo[left][right];
    }
}
```
### 877. 石子游戏
![ts](https://img-blog.csdnimg.cn/914214f411bd4d17b65e1e8b63872eb1.png)
```java
class Solution {
    public boolean stoneGame(int[] piles) {
        // 剩余 [left,right] 个石子堆时，在双方都做最好选择的情况下，先手比后手多的最大石子个数（即先手与后手的最大得分差值）
        int[][] dp = new int[piles.length][piles.length];
        /** 比如：[5,3,2,4]
        假设当 i=0,j=3 时，Alice有两种选择：
        (1)一种是选择最左边的5，然后由于下一轮对手是先手，因此对手可以在区间[1,3]中做最优决策，对手能赢我的最大差值为dp[1][3]，但是该分数对我来说是负数，因此dp[i][j]=piles[0]-dp[1][3]
        (2)一种是选择最右边的4，然后由于下一轮对手是先手，因此对手可以在区间[0,2]中做最优决策，对手能赢我的最大差值为dp[0][2]，但是该分数对我来说是负数，因此dp[i][j]=piles[3]-dp[0][2]
        因此状态转移方程就是 dp[i][j]=Math.max(piles[i]-dp[i+1][j], piles[j]-dp[i][j-1])
         */
        for(int i = 0; i < piles.length; i++){
            dp[i][i] = piles[i];
        }
        for(int i = piles.length - 1; i >= 0; i--){
            for(int j = i + 1; j < piles.length; j++){
                dp[i][j] = Math.max(piles[i] - dp[i + 1][j],
                                    piles[j] - dp[i][j - 1]);
            }
        }
        return dp[0][piles.length - 1] > 0;
    }
}
```
```java
class Solution {
    public boolean stoneGame(int[] piles) {
        /* 以[2, 1, 9, 5]为例，将这四堆石头分为2组，即1、3奇数组和2、4偶数组，那么这两组石头的数量一定不同。
        因为石头的总数是奇数，不能被平分。而作为第一个拿石头的人，你可以控制自己拿到所有的偶数堆或者所有的奇数堆 */
        return true;
    }
}
```