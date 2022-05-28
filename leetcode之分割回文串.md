

131. 分割回文串
给定一个字符串 s，将 s 分割成一些子串，使每个子串都是回文串。
返回 s 所有可能的分割方案。
示例:
输入: "aab"
输出:
[ ["aa","b"], ["a","a","b"] ]
思路：
其实切割问题类似组合问题
例如对于字符串abcdef：
组合问题：选取一个a之后，在bcdef中再去选取第二个，选取b之后在cdef中在选取第三个.....。
切割问题：切割一个a之后，在bcdef中再去切割第二段，切割b之后在cdef中在切割第三段.....。
所以切割问题也可以抽象成为树形结构：
![思路](https://img-blog.csdnimg.cn/20201204153555796.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)

递归用来纵向遍历，for循环用来横向遍历，切割线（也就是图中红线）切割到字符串的结尾位置，说明找到了一个切割方法
```java
class Solution {
    List<List<String>> res = new ArrayList<>();//存放结果
    List<String> list = new ArrayList<>();//存放切割后回文的子串
    public List<List<String>> partition(String s) {
        backtracking(s, 0);
        return res;
    }
    //递归函数参数
    public void backtracking(String s, int startIndex){
        //递归函数终止条件
        //切割线切到了字符串最后面，说明找到了一种切割方法，此时就是本层递归的终止条件
        if(startIndex == s.length()){
            res.add(new ArrayList<>(list));
            return;
        }
        //单层搜索的逻辑
        //[startIndex, i]就是我们要截取的子串
        for(int i = startIndex; i < s.length(); i++){
            String temp = s.substring(startIndex, i + 1);//获取[startIndex, i]在s中的子串
            if(judge(temp)){//判断是否为回文子串
                list.add(temp);
            }else continue;//如果不是则直接跳过
            backtracking(s, i + 1);//寻找以 i + 1为起始位置的子串。传入 i + 1是因为切割过的位置不能重复切割
            list.remove(list.size() - 1);//回溯
        }
    }
    public boolean judge(String s){
        for(int i = 0, j = s.length() - 1; i < s.length() / 2; i++, j--){
            if(s.charAt(i) != s.charAt(j)){
                return false;
            }
        }
        return true;
    }
}
```