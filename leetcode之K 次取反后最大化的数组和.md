

1005. K 次取反后最大化的数组和
给定一个整数数组 A，我们只能用以下方法修改该数组：我们选择某个索引 i 并将 A[i] 替换为 -A[i]，然后总共重复这个过程 K 次。（我们可以多次选择同一个索引 i。）
以这种方式修改数组后，返回数组可能的最大和。
示例 1：
输入：A = [4,2,3], K = 1
输出：5
解释：选择索引 (1,) ，然后 A 变为 [4,-2,3]。
示例 2：
输入：A = [2,-3,-1,5,-4], K = 2
输出：13
解释：选择索引 (1, 4) ，然后 A 变为 [2,3,-1,5,4]。
提示：
1 <= A.length <= 10000
1 <= K <= 10000
-100 <= A[i] <= 100

自己写的暴力解法：
```java
class Solution {
    public int largestSumAfterKNegations(int[] A, int K) {
        int i = 1;
        while(i <= K){
            int index = getMinIndex(A);
            A[index] = -A[index];
            i++;
        }
        int res = 0;
        for(i = 0; i < A.length; i++){
            res += A[i];
        }
        return res;
    }

    public int getMinIndex(int[] A){
        int min = A[0];
        int index = 0;
        for(int i = 0; i < A.length; i++){
            if(min > A[i]){
                min = A[i];
                index = i;
            }
        }
        return index;
    }
}
```
贪心算法思路：
局部最优：让绝对值大的负数变成正数
整体最优：整个数组和达到最大
那么如果将负数都转变成正数了，K依然大于0，此时的问题就变成了：让一个有序正整数序列，如何转变K次正负，让数组和达到最大
局部最优：只找数值最小的正整数进行反转，当前数值可以达到最大
全局最优：整个数组和达到最大

因此本题解题步骤：
第一步：将数组按绝对值大小从大到小排序
第二步：从前向后遍历，遇到负数将其变为正数，同时 K--
第三步：如果 K 还没用完，那就挑最小的那个数字反转，直到K用完为止
第四步：求和

```java
class Solution {
    public int largestSumAfterKNegations(int[] A, int K) {
        //将数组按绝对值大小从大到小排序
        quickSort(A, 0, A.length - 1);
        //从前向后遍历，遇到负数将其变为正数，同时 K--
        for(int i = 0; i < A.length; i++){
            if(A[i] < 0 && K > 0){
                A[i] *= -1;
                K--;
            }
        }
        //如果 K 还没用完，那就挑最小的那个数字反转，直到K用完为止
        if(K % 2 == 1){
            A[A.length - 1] *= -1;
        }
        //求和
        int res = 0;
        for(int num : A){
            res += num;
        }
        return res;
    }
    //快排，将数组按绝对值大小从大到小排序
    public void quickSort(int[] A, int low, int high){
        int l = low;
        int h = high;
        if(l >= h){
            return;
        }
        int num = A[l];
        int val = Math.abs(A[l]);
        while(l < h){
            while(l < h && Math.abs(A[h]) < val){
                h--;
            }
            A[l] = A[h];
            while(l < h && Math.abs(A[l]) >= val){
                l++;
            }
            A[h] = A[l];
        }
        A[l] = num;
        quickSort(A, low, h - 1);
        quickSort(A, h + 1, high);
    }
}
```