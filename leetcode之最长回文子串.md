

5. 最长回文子串
给你一个字符串 s，找到 s 中最长的回文子串。

示例 1：
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。

示例 2：
输入：s = "cbbd"
输出："bb" 

提示：
1 <= s.length <= 1000
s 仅由数字和英文字母（大写和/或小写）组成

双指针法。**寻找回文串的问题的核心思想是：从中间开始向两边扩散来判断回文串**

注意：回文串的长度可能是偶数也可能是奇数，如果是abba这种情况则会有两个中心字符。
```java
class Solution {
    public String longestPalindrome(String s) {
        String res = "";
        for(int i = 0; i < s.length(); i++){
            String s1 = palindrome(s, i, i);//找到以s.charAt(i)为中心的回文串
            String s2 = palindrome(s, i, i + 1);//找到以s.charAt(i)和s.charAt(i+1)为中心的回文串
            //res = longest(res, s1, s2)
            res = res.length() > s1.length() ? res : s1;
            res = res.length() > s2.length() ? res : s2;
        }
        return res;
    }
    public String palindrome(String s, int l, int r){//得到以s.charAt(l)和s.charAt(r)为中心的回文串
        //防止索引越界
        while(l >= 0 && r < s.length() && s.charAt(l) == s.charAt(r)){
            //向两边扩散
            l--;
            r++;
        }
        return s.substring(l + 1, r);
    }
}
```