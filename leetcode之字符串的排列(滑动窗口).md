

567. 字符串的排列
给定两个字符串 s1 和 s2，写一个函数来判断 s2 是否包含 s1 的排列。

换句话说，第一个字符串的排列之一是第二个字符串的子串。

示例 1：
输入: s1 = "ab" s2 = "eidbaooo"
输出: True
解释: s2 包含 s1 的排列之一 ("ba").

示例 2：
输入: s1= "ab" s2 = "eidboaoo"
输出: False

提示：
输入的字符串只包含小写字母
两个字符串的长度都在 [1, 10,000] 之间

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        Map<Character, Integer> need = new HashMap<>();
        Map<Character, Integer> window = new HashMap<>();
        for(int i = 0; i < s1.length(); i++){
            char c = s1.charAt(i);
            need.put(c, need.getOrDefault(c, 0) + 1);
        }

        int right = 0, left = 0;
        int valid = 0;
        while(right < s2.length()){
            char c = s2.charAt(right);
            right++;
            //进行窗口内数据的一系列更新
            if(need.containsKey(c)){
                window.put(c, window.getOrDefault(c, 0) + 1);
                if(window.get(c).equals(need.get(c))){
                    valid++;
                }
            }
            //判断左侧窗口是否要收缩
            while(right - left >= s1.length()){
                if(valid == need.size()){
                    return true;
                }
                char d = s2.charAt(left);
                left++;
                //进行窗口内数据的一系列更新
                if(need.containsKey(d)){
                    if(window.get(d).equals(need.get(d))){
                        valid--;
                    }
                    window.put(d, window.get(d) - 1);
                }
            }
        }
        return false;
    }
}
```
对于这道题的解法代码，基本上和 76.最小覆盖子串 一模一样，只需要改变两个地方：

1、本题移动left缩小窗口的时机是窗口大小大于t.size()时，因为排列嘛，显然长度应该是一样的。

2、当发现valid == need.size()时，就说明窗口中就是一个合法的排列，所以立即返回true。

至于如何处理窗口的扩大和缩小，和 76.最小覆盖子串 完全相同。

注意：本题必须用equals，而不能用==，因为在Map里存储的是Integer类型而不是int类型，Integer的[-128, 127]会缓存到整数型常量池，地址是相同的；而超出此范围的话会自动new对象，导致地址不同。