

```java
/**
 * leetcode : https://leetcode-cn.com/problems/spiral-matrix-ii/
 * 59. 螺旋矩阵 II
 * 反馈
 * 给定一个正整数 n，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。
 *
 * 示例:
 * 输入: 3
 * 输出:
 * [
 *  [ 1, 2, 3 ],
 *  [ 8, 9, 4 ],
 *  [ 7, 6, 5 ]
 * ]
 */

import java.util.Arrays;
public class Ti14 {
    public static void main(String[] args) {
        int[][] res = generateMatrix(3);
        for(int[] arr : res){
            System.out.println(Arrays.toString(arr));
        }
    }
    public static int[][] generateMatrix(int n) {
        int[][] res = new int[n][n];
        int left = 0, right = n - 1, top = 0, bottom = n - 1;
        int num = 1;
        while(num <= n * n){
            for(int i = left; i <= right; i++) res[top][i] = num++; // left to right.
            top++;
            for(int i = top; i <= bottom; i++) res[i][right] = num++; // top to bottom.
            right--;
            for(int i = right; i >= left; i--) res[bottom][i] = num++; // right to left.
            bottom--;
            for(int i = bottom; i >= top; i--) res[i][left] = num++; // bottom to top.
            left++;
        }
        return res;
    }
}
```
进阶题：
54. 螺旋矩阵
给定一个包含 m x n 个元素的矩阵（m 行, n 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。
示例 1:
输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
输出: [1,2,3,6,9,8,7,4,5]
示例 2:
输入:
[
    [1, 2, 3, 4],
    [5, 6, 7, 8],
    [9,10,11,12]
]
输出: [1,2,3,4,8,12,11,10,9,5,6,7]

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        List<Integer> res = new ArrayList<>();
        if(m == 1 && n == 1){
            res.add(matrix[0][0]);
            return res;
        }
        int left = 0, right = n - 1;
        int top = 0, bottom = m - 1;
        while(true){
            for(int i = left; i <= right; i++){
                res.add(matrix[top][i]);
            }
            top++;
            if(top > bottom){
                break;
            }
            for(int i = top; i <= bottom; i++){
                res.add(matrix[i][right]);
            }
            right--;
            if(left > right){
                break;
            }
            for(int i = right; i >= left; i--){
                res.add(matrix[bottom][i]);
            }
            bottom--;
            if(top > bottom){
                break;
            }
            for(int i = bottom; i >= top; i--){
                res.add(matrix[i][left]);
            }
            left++;
            if(left > right){
                break;
            }
        }
        return res;
    }
}
```