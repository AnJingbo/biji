

17. 电话号码的字母组合
给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。
给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。
![电话号码](https://img-blog.csdnimg.cn/20201127172344878.png)
示例:
输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
本题需解决下面三个问题：
1、数字和字母如何映射
2、两个字母就两个for循环，三个字符我就三个for循环，以此类推，然后发现代码根本写不出来(所以需要回溯算法)
3、输入1 * #按键等等异常情况，面试时一定要注意异常情况
![回溯的树的表示形式](https://img-blog.csdnimg.cn/2020112717265816.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
图中可以看出遍历的深度，就是输入"23"的长度，而叶子节点就是我们要收集的结果，输出["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]。
```java
class Solution {
    List<String> res = new ArrayList<>();//保存结果
    String s = "";//收集叶子结点的结果(由图可以看出，"23"的长度就是遍历的深度，叶子结点就是我们要收集的结果)
    String[] letterArray = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    public List<String> letterCombinations(String digits) {
        if(digits.length() == 0){
            return res;
        }
        backtracking(digits, 0);
        return res;
    }
    //确定回溯函数的参数
    //index用来记录遍历digits的第几个数字了，同时index也表示树的深度
    public void backtracking(String digits, int index){
        //确定终止条件
        //例如输入"23",那么根结点往下递归两层就可以了，叶子结点就是要收集的结果集
        //那么终止条件就是index 等于 输入的数字个数(digits.length())
        if(digits.length() == index){
            res.add(s);
            return;
        }
        //确定单层逻辑
        int digit = digits.charAt(index) - 48;//将index指向的数字转化成int
        String letter = letterArray[digit];//取数字对应的字符集
        for(int i = 0; i < letter.length(); i++){
            s += letter.charAt(i);//处理
            backtracking(digits, index + 1);//递归，注意index + 1，下一层要处理下一个数字了
            s = s.substring(0, s.length() - 1);//回溯
        }
    }
}
```