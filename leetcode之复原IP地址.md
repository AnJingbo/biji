

93. 复原IP地址
给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。
有效的 IP 地址 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 0），整数之间用 '.' 分隔。
例如："0.1.2.201" 和 "192.168.1.1" 是 有效的 IP 地址，但是 "0.011.255.245"、"192.168.1.312" 和 "192.168@1.1" 是 无效的 IP 地址。
示例 ：
输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]

切割问题可以通过回溯搜索法把所有可能搜索出来
切割问题可以抽象为树形结构：
![图解](https://img-blog.csdnimg.cn/20201202202228155.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)

```java
class Solution {
    List<String> res = new ArrayList<>();//记录结果
    List<String> list = new ArrayList<>();//记录每一段的字符串的值
    public List<String> restoreIpAddresses(String s) {
        if(s.length() < 4){
            return new ArrayList<>();
        }
        backtracking(s, 0);
        return res;
    }
    //递归参数
    public void backtracking(String s, int startIndex){
        //递归终止条件
        //当一共有四段的时候，终止递归
        if(list.size() == 4){
            if(startIndex == s.length()){//当startIndex等于s的长度的时候，表明该结果有效，可以加入res中
                String str = "";
                for(int i = 0; i < list.size(); i++){
                    str += list.get(i) + ".";
                }
                str = str.substring(0, str.length() - 1);
                res.add(str);
            }
            return;
        }
        //单层搜索的逻辑
        for(int i = startIndex; i < s.length(); i++){
            String temp = s.substring(startIndex, i + 1);
            if(isValid(temp)){//判断temp是否合法
                list.add(temp);//temp合法则将temp加入list中
                backtracking(s, i + 1);
                list.remove(list.size() - 1);//回溯
            }else break;//不合法，直接结束循环
        }
    }

    public boolean isValid(String s){//判断字符串s是否合法
        if(s.length() <= 0 || s.length() >= 4){//字符串长度小于等于0或者大于等于4
            return false;
        }
        if(s.length() >= 2 && s.charAt(0) == '0'){//字符串长度大于1并且首字母为0
            return false;
        }
        int num = 0;
        for(int i = s.length() - 1, x = 1; i >= 0; i--, x *= 10){
            num += (s.charAt(i) - 48) * x;
        }
        if(num > 255 || num < 0){
            return false;
        }
        return true;
    }
}
```