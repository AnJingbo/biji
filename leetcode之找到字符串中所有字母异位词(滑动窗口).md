

438. 找到字符串中所有字母异位词
给定一个字符串 s 和一个非空字符串 p，找到 s 中所有是 p 的字母异位词的子串，返回这些子串的起始索引。

字符串只包含小写英文字母，并且字符串 s 和 p 的长度都不超过 20100。

说明：

字母异位词指字母相同，但排列不同的字符串。
不考虑答案输出的顺序。
示例 1:

输入:
s: "cbaebabacd" p: "abc"

输出:
[0, 6]

解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的字母异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的字母异位词。
 示例 2:

输入:
s: "abab" p: "ab"

输出:
[0, 1, 2]

解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的字母异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的字母异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的字母异位词。

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        HashMap<Character, Integer> need = new HashMap<>();
        HashMap<Character, Integer> window = new HashMap<>();
        
        for(int i = 0; i < p.length(); i++){
            char c = p.charAt(i);
            need.put(c, need.getOrDefault(c, 0) + 1);
        }
        List<Integer> res = new ArrayList<>();//记录结果

        int right = 0, left = 0;
        int valid = 0;
        while(right < s.length()){//进行窗口内数据的一系列更新
            char c = s.charAt(right);
            right++;
            if(need.containsKey(c)){
                window.put(c, window.getOrDefault(c, 0) + 1);

                if(need.get(c).equals(window.get(c))){
                    valid++;
                }
            }

            while(right - left == p.length()){//判断左侧窗口是否要收缩
                if(valid == need.size()){//当窗口符合条件时，把起始索引加入res
                    res.add(left);
                }
                char d = s.charAt(left);
                left++;
                //注：和上面是反着的。
                if(need.containsKey(d)){//进行窗口内数据的一系列更新
                    if(need.get(d).equals(window.get(d))){
                        valid--;
                    }
                    window.put(d, window.get(d) - 1);
                }
            }
        }
        return res;
    }
}
```