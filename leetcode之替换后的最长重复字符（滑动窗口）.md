

### 424. 替换后的最长重复字符
![ts](https://img-blog.csdnimg.cn/82005f501d114337949193cc63fd2536.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public int characterReplacement(String s, int k) {
        int left = 0, right = 0;
        int[] alphabet = new int[26];//记录当前窗口中字符出现的次数
        int maxLen = 0; // 当前窗口范围内相同字符出现的最大次数
        int res = 0;
        while(right < s.length()){
            char c = s.charAt(right);
            right++;
            alphabet[c - 'A']++;
            maxLen = Math.max(maxLen, alphabet[c - 'A']);
            // 窗口大小(窗口内字符的个数) - 窗口内相同字符出现的最大次数 > 可以修改的次数
            while(right - left - maxLen > k){
                char d = s.charAt(left);
                left++;
                alphabet[d - 'A']--;
            }
            res = Math.max(res, right - left);
        }
        return res;
    }
}
```