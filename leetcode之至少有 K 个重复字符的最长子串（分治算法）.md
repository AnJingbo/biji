

![图示](https://img-blog.csdnimg.cn/20210608094617461.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
思路：
先整体考虑，如果某个字符在整个字符串中的出现次数 < k，那它一定不会出现在合法子串中。因此需要以它为分界点，到它的两侧找。
然后情况就会变成一个规模较小的子问题，递归去求左侧和右侧中合法子串的最长长度。
当递归到子问题的规模足够小，即子串的长度小于 k，即便子串的字符都相同，字符的出现次数也小于 k，所以没有合法子串，返回 0

```java
class Solution {
    public int longestSubstring(String s, int k) {
        if(s.length() < k){
            return 0;
        }
        HashMap<Character, Integer> map = new HashMap<>();
        for(int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            map.put(c, map.getOrDefault(c, 0) + 1);
        }
        int res = 0;
        for(int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            if(map.get(c) < k){// 如果字符 c 的出现次数小于 k，则需要切割
                int left = longestSubstring(s.substring(0, i), k);
                int right = longestSubstring(s.substring(i + 1, s.length()), k);
                res = Math.max(left, right);
                return res;
            }
        }
        // 如果没有切割，也就是 s 中所有字符的出现次数都大于 k，说明 s 字符串整个都是符合要求的，则直接返回 s 的长度
        return s.length();
    }
}
```