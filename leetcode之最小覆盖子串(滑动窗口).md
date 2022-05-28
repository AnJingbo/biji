

76. 最小覆盖子串
给你一个字符串 s 、一个字符串 t 。返回 s 中涵盖 t 所有字符的最小子串。如果 s 中不存在涵盖 t 所有字符的子串，则返回空字符串 "" 。

注意：如果 s 中存在这样的子串，我们保证它是唯一的答案。

示例 1：
输入：s = "ADOBECODEBANC", t = "ABC"
输出："BANC"
示例 2：
输入：s = "a", t = "a"
输出："a"

提示：
1 <= s.length, t.length <= 105
s 和 t 由英文字母组成

滑动窗口算法的思路是这样：

1、我们在字符串S中使用双指针中的左右指针技巧，初始化left = right = 0，**把索引左闭右开区间[left, right)称为一个「窗口」。**

2、我们先不断地增加right指针扩大窗口[left, right)，直到窗口中的字符串符合要求（包含了T中的所有字符）。

3、此时，我们停止增加right，转而不断增加left指针缩小窗口[left, right)，直到窗口中的字符串不再符合要求（不包含T中的所有字符了）。同时，每次增加left，我们都要更新一轮结果。

4、重复第 2 和第 3 步，直到right到达字符串S的尽头。

这个思路其实也不难，**第 2 步相当于在寻找一个「可行解」，然后第 3 步在优化这个「可行解」**，最终找到最优解，也就是最短的覆盖子串。左右指针轮流前进，窗口大小增增减减，窗口不断向右滑动，这就是「滑动窗口」这个名字的来历。

**need和window相当于计数器，分别记录T中字符出现次数和「窗口」中的相应字符的出现次数**。

首先，初始化window和need两个哈希表，记录窗口中的字符和需要凑齐的字符：
```java
HashMap<Character, Integer> need = new HashMap<>();
HashMap<Character, Integer> window = new HashMap<>();
int left = 0, right = 0;
int valid = 0;//窗口中满足need条件的字符个数
int start = 0, len = Integer.MAX_VALUE;
for(int i = 0; i < t.length(); i++){
    char c = t.charAt(i);
    if(need.containsKey(c)){
        need.put(c, need.get(c) + 1);
    }else{
        need.put(c, 1);
    }
}
```

然后，使用left和right变量初始化窗口的两端，不要忘了，区间[left, right)是左闭右开的，所以初始情况下窗口没有包含任何元素：

```java
int left = 0, right = 0;
int valid = 0; 
while (right < s.length()) {
    // 开始滑动
}
```

其中**valid变量表示窗口中满足need条件的字符个数**，如果valid和need.size的大小相同，则说明窗口已满足条件，已经完全覆盖了串T。

现在开始套模板，只需要思考以下四个问题：

1、当移动right扩大窗口，即加入字符时，应该更新哪些数据？

2、什么条件下，窗口应该暂停扩大，开始移动left缩小窗口？

3、当移动left缩小窗口，即移出字符时，应该更新哪些数据？

4、我们要的结果应该在扩大窗口时还是缩小窗口时进行更新？

如果一个字符进入窗口，应该增加window计数器；如果一个字符将移出窗口的时候，应该减少window计数器；当valid满足need时应该收缩窗口；应该在收缩窗口的时候更新最终结果。

注意：本题必须用equals，而不能用==，因为在Map里存储的是Integer类型而不是int类型，Integer的[-128, 127]会缓存到整数型常量池，地址是相同的；而超出此范围的话会自动new对象，导致地址不同。
```java
class Solution {
    public String minWindow(String s, String t) {
        HashMap<Character, Integer> need = new HashMap<>();
        HashMap<Character, Integer> window = new HashMap<>();
        int left = 0, right = 0;
        int valid = 0;//窗口中满足need条件的字符个数
        int start = 0, len = Integer.MAX_VALUE;//记录最小覆盖子串的起始索引以及长度
        for(int i = 0; i < t.length(); i++){
            char c = t.charAt(i);
            if(need.containsKey(c)){
                need.put(c, need.get(c) + 1);
            }else{
                need.put(c, 1);
            }
        }
        while(right < s.length()){
            char c = s.charAt(right);//c是将移入窗口的字符
            right++;//右移窗口
            //进行窗口内数据的一系列更新
            if(need.containsKey(c)){

                if(window.containsKey(c)){
                    window.put(c, window.get(c) + 1);
                }else{
                    window.put(c, 1);
                }
                
                if(window.get(c).equals(need.get(c))){
                    valid++;
                }
            }
            //判断左侧窗口是否要收缩
            while(valid == need.size()){
                //在这里更新最小覆盖字串
                if(right - left < len){
                    start = left;
                    len = right - left;
                }
                char d = s.charAt(left);//d是将移出窗口的字符
                left++;//左移窗口
                //进行窗口内数据的一系列更新
                if(need.containsKey(d)){
                    if(window.get(d).equals(need.get(d))){
                        valid--;
                    }
                    window.put(d, window.get(d) - 1);
                }
            }
        }
        return len == Integer.MAX_VALUE ? "" : s.substring(start, start + len);//返回最小覆盖子串
    }
}
```
使用JDK8中的getOrDefault方法之后：

语法：map.getOrDefault(key,defaultValue);
当map中存在key时，输出对应的value
当map中不存在key时，输出defaultValue

```java
class Solution {
    public String minWindow(String s, String t) {
        HashMap<Character, Integer> need = new HashMap<>();
        HashMap<Character, Integer> window = new HashMap<>();
        int left = 0, right = 0;
        int valid = 0;//窗口中满足need条件的字符个数
        int start = 0, len = Integer.MAX_VALUE;//记录最小覆盖子串的起始索引以及长度
        for(int i = 0; i < t.length(); i++){
            char c = t.charAt(i);
            need.put(c, need.getOrDefault(c, 0) + 1);
        }
        while(right < s.length()){
            char c = s.charAt(right);//c是将移入窗口的字符
            right++;//右移窗口
            //进行窗口内数据的一系列更新
            if(need.containsKey(c)){
                window.put(c, window.getOrDefault(c, 0) + 1);
                
                if(window.get(c).equals(need.get(c))){
                    valid++;
                }
            }
            //判断左侧窗口是否要收缩
            while(valid == need.size()){
                //在这里更新最小覆盖字串
                if(right - left < len){
                    start = left;
                    len = right - left;
                }
                char d = s.charAt(left);//d是将移出窗口的字符
                left++;//左移窗口
                //进行窗口内数据的一系列更新
                if(need.containsKey(d)){
                    if(window.get(d).equals(need.get(d))){
                        valid--;
                    }
                    window.put(d, window.get(d) - 1);
                }
            }
        }
        return len == Integer.MAX_VALUE ? "" : s.substring(start, start + len);//返回最小覆盖子串
    }
}
```
```java
class Solution {
    public String minWindow(String s, String t) {
        HashMap<Character, Integer> need = new HashMap<>();
        HashMap<Character, Integer> window = new HashMap<>();
        for(int i = 0; i < t.length(); i++){
            need.put(t.charAt(i), need.getOrDefault(t.charAt(i), 0) + 1);
        }
        int left = 0, right = 0;
        int valid = 0;
        int start = 0, len = Integer.MAX_VALUE;
        while(right < s.length()){
            char c = s.charAt(right);
            right++;

            window.put(c, window.getOrDefault(c, 0) + 1);
            if(window.get(c).equals(need.get(c))){
                valid++;
            }
            
            while(valid == need.size()){
                if(right - left < len){
                    start = left;
                    len = right - left;
                }
                char d = s.charAt(left);
                left++;

                if(window.get(d).equals(need.get(d))){
                    valid--;
                }
                window.put(d, window.get(d) - 1);
            }
        }
        return len == Integer.MAX_VALUE ? "" : s.substring(start, start + len);
    }
}
```
需要注意的是，当我们发现某个字符在window的数量满足了need的需要，就要更新valid，表示有一个字符已经满足要求。而且，你能发现，两次对窗口内数据的更新操作是完全对称的。

当valid == need.size()时，说明T中所有字符已经被覆盖，已经得到一个可行的覆盖子串，现在应该开始收缩窗口了，以便得到「最小覆盖子串」。

移动left收缩窗口时，窗口内的字符都是可行解，所以应该在收缩窗口的阶段进行最小覆盖子串的更新，以便从可行解中找到长度最短的最终结果。

滑动窗口模板：
```cpp
/* 滑动窗口算法框架 */
void slidingWindow(string s, string t) {
    unordered_map<char, int> need, window;
    for (char c : t) need[c]++;

    int left = 0, right = 0;
    int valid = 0; 
    while (right < s.size()) {
        // c 是将移入窗口的字符
        char c = s[right];
        // 右移窗口
        right++;
        // 进行窗口内数据的一系列更新
        ...

        /*** debug 输出的位置 ***/
        printf("window: [%d, %d)\n", left, right);
        /********************/

        // 判断左侧窗口是否要收缩
        while (window needs shrink) {
            // d 是将移出窗口的字符
            char d = s[left];
            // 左移窗口
            left++;
            // 进行窗口内数据的一系列更新
            ...
        }
    }
}
```

其中两处…表示的更新窗口数据的地方，到时候直接往里面填就行了。

而且，这两个…处的操作分别是右移和左移窗口更新操作，等会你会发现它们操作是完全对称的。