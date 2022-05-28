

```java
import java.util.Arrays;

public class KMP {
    public static void main(String[] args) {
        String s1 = "aabaabaafa";
        String s2 = "aabaaf";

        int[] next = kmpNext(s2);
        System.out.println(Arrays.toString(next));

        int index = kmpSearch(s1, s2, next);
        System.out.println(index);
    }

    /**
     * @param s1   原字符串
     * @param s2   子串
     * @param next 部分匹配表，是字串对应的部分匹配表
     * @return 如果是-1就是没有匹配到，否则返回第一个匹配表的位置
     */
    public static int kmpSearch(String s1, String s2, int[] next) {
        for (int i = 0, j = 0; i < s1.length(); i++) {
            //需要处理当s1.charAt(i) != s2.charAt(j)
            //KMP算法核心点
            while (j > 0 && s1.charAt(i) != s2.charAt(j)) {
                j = next[j - 1];
            }
            if (s1.charAt(i) == s2.charAt(j)) {
                j++;
            }
            if (j == s2.length()) {
                return i - j + 1;
            }
        }
        return -1;
    }

    // 获取一个字符串(子串)的部分匹配值表
    public static int[] kmpNext(String dest) {
        int[] next = new int[dest.length()];
        next[0] = 0;// 如果字符串的长度为1，那么他的部分匹配值一定是0
        // 下标i可以理解为前i+1个字符的匹配值
        // 比如：i=0时代表a的匹配值，i=1时代表aa的匹配值，i=2时代表aab的匹配值，i=3代表aaba的匹配值等等，因此是next[i] = j;
        for (int i = 1, j = 0; i < next.length; i++) {
            // 当dest.charAt(i) != dest.charAt(j),我们需要从a[j-1]获取新的j
            // 直到我们发现有dest.charAt(i) == dest.charAt(j)成立才退出
            // 这是KMP算法的核心点
            while (j > 0 && dest.charAt(i) != dest.charAt(j)) {
                j = next[j - 1];
            }
            // 当dest.charAt(i) == dest.charAt(j)时，部分匹配值就需要+1
            if (dest.charAt(i) == dest.charAt(j)) {
                j++;
            }
            next[i] = j;
        }
        return next;
    }
     public static void baoLi(String s1, String s2){
        int i = 0;
        int j = 0;
        while(i < s1.length() && j < s2.length()){
            if(s2.charAt(j) == s1.charAt(i)){
                i++;
                j++;
            }else{
            // 将i指向本次匹配的开始位置的下一个字符
                i = i - j + 1;
                j = 0;
            }
        }
        if(j == s2.length()){
            System.out.println("找到该字符串，其首字母的下标为" + (i - j));
        }else{
            System.out.println("没有找到该字符串");
        }
    }
}
```