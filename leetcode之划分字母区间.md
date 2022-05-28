

763. 划分字母区间
字符串 S 由小写字母组成。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。返回一个表示每个字符串片段的长度的列表。
示例：
输入：S = "ababcbacadefegdehijhklij"
输出：[9,7,8]
解释：
划分结果为 "ababcbaca", "defegde", "hijhklij"。
每个字母最多出现在一个片段中。
像 "ababcbacadefegde", "hijhklij" 的划分是错误的，因为划分的片段数较少。
提示：S的长度在[1, 500]之间。
S只包含小写字母 'a' 到 'z' 。

思路：
在遍历过程中相当于是要找每一个字母的边界，如果找到之前遍历过的所有字母的最远边界，说明这个边界就是分割点了。此时前面出现过的所有字母，最远也就到这个边界了。
可以分为以下两个步骤：
1、统计每一个字符最后出现的位置。
2、从头遍历字符，并更新字符最远出现下标，如果发现当前下标和字符最远出现位置的下标相等了，则找到了分割点。![图示](https://img-blog.csdnimg.cn/20210103192014656.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)

```java
class Solution {
    public List<Integer> partitionLabels(String S) {
        List<Integer> res = new ArrayList<>();
        int[] last = new int[27];// last 数组存储字符出现的最后位置
        for(int i = 0; i < S.length(); i++){//统计每一个字符最后出现的位置
            last[S.charAt(i) - 'a'] = i;
        }
        int left = 0;
        int right = 0;
        for(int i = 0; i < S.length(); i++){
            right = Math.max(right, last[S.charAt(i) - 'a']);//找到字符出现的最远位置
            if(i == right){
                res.add(right - left + 1);
                left = i + 1;
            }
        }
        return res;
    }
}
```