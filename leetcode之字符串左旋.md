

```java
/**
 * 剑指 Offer 58 - II. 左旋转字符串
 * 字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。
 *
 * 示例 1：
 * 输入: s = "abcdefg", k = 2
 * 输出: "cdefgab"
 * 示例 2：
 * 输入: s = "lrloseumgh", k = 6
 * 输出: "umghlrlose"
 *
 */
public class Ti26 {
    /*
    不申请额外空间，只在本串上操作思路：
      通过整体反转+局部反转的方式实现左旋转
        1、反转区间为前n的字串：bacdefg
        2、反转区间为n到末尾的字串：bagfedc
        3、反转整个字符串：cdefgab
     */
    public static void main(String[] args) {
        String res = reverseLeftWords("abcdefg", 2);
        System.out.println(res);
    }
    public static String reverseLeftWords(String s, int n) {
        char[] chars = s.toCharArray();
        reverse(chars, 0, n - 1);
        reverse(chars, n, chars.length - 1);
        reverse(chars, 0, chars.length - 1);
        return new String(chars);
    }
    public static void reverse(char[] chars, int start, int end){
                                 //start + (end - start + 1) / 2
       for(int i = start, j = end; i < (start + end + 1) / 2; i++, j--){
            char temp = chars[i];
            chars[i] = chars[j];
            chars[j] = temp;
       }
       /*
      for(int i = start, j = end; i <= (start + end) / 2; i++, j--){
            char temp = chars[i];
            chars[i] = chars[j];
            chars[j] = temp;
      }*/
   }
}
```