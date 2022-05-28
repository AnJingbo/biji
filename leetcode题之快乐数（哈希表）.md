

```java
import java.util.HashSet;

/**
 * 202、快乐数
 * 编写一个算法来判断一个数 n 是不是快乐数。
 * 「快乐数」定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和，然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。如果 可以变为  1，那么这个数就是快乐数。
 * 如果 n 是快乐数就返回 True ；不是，则返回 False 。
 * <p>
 * 示例：
 * 输入：19
 * 输出：true
 * 解释：
 * 1^2 + 9^2 = 82
 * 8^2 + 2^2 = 68
 * 6^2 + 8^2 = 100
 * 1^2 + 0^2 + 0^2 = 1
 * <p>
 * 链接：https://leetcode-cn.com/problems/happy-number
 */
public class Ti20 {
    public static void main(String[] args) {
        System.out.println(isHappy(9));
    }

    //题中说了会无限循环，那麽也就是说求和的过程中，sum会重复出现！
    //当我们遇到了要快速判断一个元素是否出现集合里的时候，就要考虑哈希法
    public static boolean isHappy(int n) {
        HashSet<Integer> hashSet = new HashSet<>();
        while (true) {
            int sum = getSum(n);
            if (sum == 1) {
                return true;
            }
            if (hashSet.contains(sum)) {
                return false;
            } else {
                hashSet.add(sum);
            }
            n = sum;
        }

    }

    public static int getSum(int n) {
        int sum = 0;
        /*for (int i = 0, x = 1; i < (n + "").length(); i++, x *= 10) {
            sum += (n / x % 10) * (n / x % 10);
        }*/
        while (n != 0) {
            sum += (n % 10) * (n % 10);
            n /= 10;
        }
        return sum;
    }
}
```