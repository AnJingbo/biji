

```java
``
/**
 * 给定两个字符串 s1 和 s2，请编写一个程序，确定其中一个字符串的字符重新排列后，能否变成另一个字符串。（假设里面只有小写字母）
 *
 * 示例 1：
 * 输入: s1 = "abc", s2 = "bca"
 * 输出: true
 * 示例 2：
 * 输入: s1 = "abc", s2 = "bad"
 * 输出: false
 *
 * 来源：力扣（LeetCode）
 * 链接：https://leetcode-cn.com/problems/check-permutation-lcci
 */
//注：出现有关字符的问题时，可以考虑关于使用 ASCII码 的方法去求解
public class Ti {
    public static void main(String[] args) {
        String s1 = "abca";
        String s2 = "baca";
        boolean b = CheckPermutation(s1, s2);
        System.out.println(b == true ? "该字符串可以变成另一个字符串" : "该字符串不能变成另一个字符串");
    }

    /*public static boolean CheckPermutation(String s1, String s2) {
        if (s1.length() != s2.length()) {
            return false;
        }

        boolean[] judge = new boolean[s1.length()];
        boolean[] a = new boolean[s1.length()];
        for (int i = 0; i < s1.length(); i++) {
            for (int j = 0; j < s2.length(); j++) {
                if (judge[j] == false && s1.charAt(i) == s2.charAt(j)) {
                    a[i] = true;//表示s1里下标为i的元素已经在s2里找到
                    judge[j] = true;//表示s2里下标为j的那个元素已经被访问过
                    break;
                }
            }
        }
        int t = 0;
        for (int i = 0; i < s1.length(); i++) {
            if (a[i] == false) {
                t = 1;
                break;
            }
        }
        if (t == 0) {
            return true;
        } else {
            return false;
        }
    }*/
    public static boolean CheckPermutation(String s1, String s2) {
        if(s1.length()!=s2.length()) return false;
        int[] nums = new int[26];
        for (int i = 0; i < s1.length(); i++) {
            nums[s1.charAt(i)-97]++;
            nums[s2.charAt(i)-97]--;
        }
        for (int num : nums) {
            if (num != 0) return false;
        }
        return true;
    }

}
```