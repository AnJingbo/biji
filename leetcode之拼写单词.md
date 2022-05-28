

```java
/**
 * 给你一份『词汇表』（字符串数组） words 和一张『字母表』（字符串） chars。
 * 假如你可以用 chars 中的『字母』（字符）拼写出 words 中的某个『单词』（字符串），那么我们就认为你掌握了这个单词。
 * 注意：每次拼写（指拼写词汇表中的一个单词）时，chars 中的每个字母都只能用一次。
 * 返回词汇表 words 中你掌握的所有单词的 长度之和。
 * <p>
 * 示例 ：
 * 输入：words = ["cat","bt","hat","tree"], chars = "atach"
 * 输出：6
 * 解释：
 * 可以形成字符串 "cat" 和 "hat"，所以答案是 3 + 3 = 6。
 * <p>
 * 链接：https://leetcode-cn.com/problems/find-words-that-can-be-formed-by-characters
 */
public class Ti13 {
    public static void main(String[] args) {
        String[] words = {"cat","bt","hat","tree"};
        String chars = "atach";
        //int num = countCharacters(words, chars);
        int num = countCharacters(new String[]{"cat","bt","hat","tree"}, chars);
        System.out.println(num);
    }
    //遇到有提示字符串仅包含小写（或者大写）英文字母的题，
    //都可以试着考虑能不能构造长度为26的每个元素分别代表一个字母的数组，来简化计算
    public static int countCharacters(String[] words, String chars) {
        int res = 0;
        int[] a = new int[26];
        for (int i = 0; i < chars.length(); i++) {
            a[chars.charAt(i) - 97] += chars.charAt(i) - 96;
        }

        for (String word : words) {
            boolean flag = true;
            int[] w = new int[26];
            for (int i = 0; i < word.length(); i++) {
                w[word.charAt(i) - 97] += word.charAt(i) - 96;
            }
            for (int j = 0; j < 26; j++) {
                if (w[j] > a[j]) {
                    flag = false;
                    break;
                }
            }
            if (flag) {
                res += word.length();
            }
        }
        return res;
    }
}
```