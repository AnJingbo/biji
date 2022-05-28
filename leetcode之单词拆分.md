

139. 单词拆分
给定一个非空字符串 s 和一个包含非空单词的列表 wordDict，判定 s 是否可以被空格拆分为一个或多个在字典中出现的单词。

说明：

拆分时可以重复使用字典中的单词。
你可以假设字典中没有重复的单词。
示例 1：

输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以被拆分成 "leet code"。
示例 2：

输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: 返回 true 因为 "applepenapple" 可以被拆分成 "apple pen apple"。
     注意你可以重复使用字典中的单词。
示例 3：

输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false


回溯：
第一种：
```java
class Solution {
    String temp = "";
    public boolean wordBreak(String s, List<String> wordDict) {
        return backtracking(s, wordDict);
    }
    
    public boolean backtracking(String s, List<String> wordDict){
        if(temp.length() > s.length()){
            return false;
        }
        if(temp.length() == s.length()){
            if(s.equals(temp)){
                return true;
            }
            return false;
        }
        for(int i = 0; i < wordDict.size(); i++){
            int len = wordDict.get(i).length();
            temp += wordDict.get(i);
            if(backtracking(s, wordDict)){
                return true;
            }
            temp = temp.substring(0, temp.length() - len);
        }
        return false;
    }
}
```
第二种：
```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        return backtracking(s, wordDict, 0);
    }

    public boolean backtracking(String s, List<String> wordDict, int startIndex){
        if(startIndex >= s.length()){
            return true;
        }
        for(int i = startIndex; i < s.length(); i++){
            String temp = s.substring(startIndex, i + 1);//切割[startIndex, i]的字符串
            if(wordDict.contains(temp) && backtracking(s, wordDict, i + 1)){
                return true;
            }
        }
        return false;
    }
}
```
**记忆化递归：递归的过程中有很多重复计算，可以使用数组保存一下递归过程中计算的结果**。

使用memory数组保存每次计算的以startIndex起始的计算结果，如果memory[startIndex]里已经被赋值了，直接用memory[startIndex]的结果。

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        int[] memory = new int[s.length()];//使用memory数组保存每次计算的以startIndex起始的计算结果
        Arrays.fill(memory, -1);//-1 表示初始化状态
        return backtracking(s, wordDict, 0, memory);
    }

    public boolean backtracking(String s, List<String> wordDict, int startIndex, int[] memory){
        if(startIndex >= s.length()){
            return true;
        }
        if(memory[startIndex] != -1){//如果memory[startIndex]不是初始值了，直接使用memory[startIndex]的结果
            return memory[startIndex] == 1;
        }
        for(int i = startIndex; i < s.length(); i++){
            String temp = s.substring(startIndex, i + 1);//切割[startIndex, i]的字符串
            if(wordDict.contains(temp) && backtracking(s, wordDict, i + 1, memory)){
                memory[startIndex] = 1;//记录以startIndex开始的子串是可以拆分的
                return true;
            }
        }
        memory[startIndex] = 0;//记录以startIndex开始的子串是不可以拆分的
        return false;
    } 
}
```

动态规划：

单词就是物品，字符串s就是背包，单词能否组成字符串s，就是问物品能不能把背包装满。

拆分时可以重复使用字典中的单词，说明就是一个完全背包！

动规五部曲分析如下：

**1、确定dp数组以及下标的含义**

dp[i] : **i之前的字符串是否可以拆分。**

**2、确定递推公式**

如果 j 之前的字符串可以被拆分，并且 [j, i) 这个区间的子串出现在字典里，那么 i 之前的字符串一定也可以被拆分，即dp[i]一定是true。（j < i) 

注：因为 i 是从1开始并且最大值是s.length()，所以即使是左闭右开，也可以覆盖整个 s 字符串。

所以递推公式是 **dp[j]是true && if([j, i) 这个区间的子串出现在字典里) 那么 dp[i] = true。**

**3、dp数组如何初始化**

从递归公式中可以看出，dp[i] 的状态依靠 dp[j]是否为true，那么dp[0]就是递归的根基，dp[0]一定要为true，否则递归下去后面都都是false了。

那么dp[0]有没有意义呢？

dp[0]表示如果字符串为空的话，说明出现在字典里。

但题目中说了“给定一个非空字符串 s” 所以测试数据中不会出现i为0的情况，那么dp[0]初始为true完全就是为了推导公式。

下标非0的dp[i]初始化为false，只要没有被覆盖说明都是不可拆分为一个或多个在字典中出现的单词。

**4、确定遍历顺序**

题目中说是拆分为一个或多个在字典中出现的单词，所以这是完全背包。

还要讨论两层for循环的前后循序。

**如果求组合数就是外层for循环遍历物品，内层for遍历背包。
如果求排列数就是外层for遍历背包，内层for循环遍历物品。**

**本题最终要求的是是否都出现过，所以对出现单词集合里的元素是组合还是排列**，并不在意！

那么**本题使用求排列的方式，还是求组合的方式都可以**。

即：外层for循环遍历物品，内层for遍历背包 或者 外层for遍历背包，内层for循环遍历物品 都是可以的。

但本题还有特殊性，因为是要求子串，最好是遍历背包放在外循环，将遍历物品放在内循环。

如果要是外层for循环遍历物品，内层for遍历背包，就需要把所有的子串都预先放在一个容器里。（如果不理解的话，可以自己尝试这么写一写就理解了）

所以最终我选择的遍历顺序为：遍历背包放在外循环，将遍历物品放在内循环。内循环从前到后。

**5、举例推导dp数组**

以输入: s = "leetcode", wordDict = ["leet", "code"]为例，dp状态如图：
![图示](https://img-blog.csdnimg.cn/20210208150221172.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        boolean[] dp = new boolean[s.length() + 1];//i之前的字符串是否可以拆分
        dp[0] = true;
        for(int i = 1; i <= s.length() ; i++){
            for(int j = 0; j < i; j++){
                String temp = s.substring(j, i);
                if(dp[j] && wordDict.contains(temp)){//j之前的字符串可以被拆分 并且 [j, i)的字符串在字典中
                    dp[i] = true;//i之前的字符串可以被拆分
                }
            }
        }
        return dp[s.length()];
    }
}
```