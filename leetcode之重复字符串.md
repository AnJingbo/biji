

```java
/**
 * 459. 重复的子字符串
 * 给定一个非空的字符串，判断它是否可以由它的一个子串重复多次构成。给定的字符串只含有小写英文字母，并且长度不超过10000。
 * 示例 1:
 * 输入: "abab"
 * 输出: True
 * 解释: 可由子字符串 "ab" 重复两次构成。
 * 示例 2:
 * 输入: "ababba"
 * 输出: False
 */
public class KMPApplication {
    public static void main(String[] args) {
        boolean flag = repeatedSubstringPattern("ababba");
        System.out.println(flag);
    }
    /*
    最长相等前后缀的长度为：next[len] - 1]
    数组长度：len
    如果len % (len - (next[len - 1] + 1)) == 0, 则说明(数组长度-最长相等前后缀的长度)正好可以被数组的长度整除，说明该字符串有重复的子字符串
    */
    public static boolean repeatedSubstringPattern(String s) {
        if(s.length() == 1){
            return false;
        }
        int[] next = getNext(s);
        int len = s.length();
        if(next[len - 1] != 0 && len % (len - (next[len - 1])) == 0){
            return true;
        }
        return false;
    }

    public static int[] getNext(String s){
        int[] next = new int[s.length()];
        next[0] = 0;
        for(int i = 1, j = 0; i < next.length; i++){
            while(j > 0 && s.charAt(i) != s.charAt(j)){
                j = next[j - 1];
            }
            if(s.charAt(i) == s.charAt(j)){
                j++;
            }
            next[i] = j;
        }
        return next;
    }
}
```