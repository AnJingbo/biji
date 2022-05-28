

```java
import java.util.Arrays;

/**
 * 给定一个按非递减顺序排序的整数数组 A，返回每个数字的平方组成的新数组，要求也按非递减顺序排序。
 * 示例 1：
 *
 * 输入：[-4,-1,0,3,10]
 * 输出：[0,1,9,16,100]
 * 示例 2：
 *
 * 输入：[-7,-3,2,3,11]
 * 输出：[4,9,9,49,121]
 *
 * 链接：https://leetcode-cn.com/problems/squares-of-a-sorted-array
 */
 /**
 * 分析：数组其实是有序的， 只不过负数平方之后可能成为最大数了。
 * 那么数组平方的最大值就在数组的两端，不是最左边就是最右边，不可能是中间。
 * 此时可以考虑双指针法了，i指向起始位置，j指向终止位置。
 * 定义一个新数组result，和A数组一样的大小，让k指向result数组终止位置。
 *
 */
public class Ti15 {
    public static void main(String[] args) {
        int[] A = {-4,-1,0,3,10};
        //int[] res = sortedSquares(A);
        int[] res = sortedSquares1(A);
        System.out.println(Arrays.toString(res));
    }
    public static int[] sortedSquares1(int[] A) {
        for(int i = 0; i < A.length; i++){
            A[i] = A[i] * A[i];
        }
        Arrays.sort(A);
        return A;
    }
    //双指针法
    public static int[] sortedSquares(int[] A) {
        int[] res = new int[A.length];
        int i = 0, j = A.length - 1, k = A.length - 1;
        while(i <= j){
            if(A[i] * A[i] < A[j] * A[j]){
                res[k--] = A[j] * A[j];
                j--;
            }else{
                res[k--] = A[i] * A[i];
                i++;
            }
        }
        return res;
    }
}
```