

842. 将数组拆分成斐波那契序列
给定一个数字字符串 S，比如 S = "123456579"，我们可以将它分成斐波那契式的序列 [123, 456, 579]。
形式上，斐波那契式序列是一个非负整数列表 F，且满足：
0 <= F[i] <= 2^31 - 1，（也就是说，每个整数都符合 32 位有符号整数类型）；
F.length >= 3；
对于所有的0 <= i < F.length - 2，都有 F[i] + F[i+1] = F[i+2] 成立。
另外，请注意，将字符串拆分成小块时，每个块的数字一定不要以零开头，除非这个块是数字 0 本身。
返回从 S 拆分出来的任意一组斐波那契式的序列块，如果不能拆分则返回 []。
示例 1：
输入："123456579"
输出：[123,456,579]
示例 2：
输入："0123"
输出：[]
解释：每个块的数字不能以零开头，因此 "01"，"2"，"3" 不是有效答案。

```java
class Solution {
    List<Integer> res = new ArrayList<>();
    public List<Integer> splitIntoFibonacci(String S) {
        backtracking(S, 0);
        return res;
    }

    public boolean backtracking(String s, int startIndex){
        if(startIndex == s.length() && res.size() >= 3){
            return true;
        }
        for(int i = startIndex; i < s.length(); i++){
            String temp = s.substring(startIndex, i + 1);//分割字符串
            if(temp.charAt(0) == '0' && temp.length() > 1){//如果数字以0开头而且不是0本身
                break;
            }
            long num = 0;
            for(int j = 0; j < temp.length(); j++){//截取字符串转化为数字，用long存储
                num = num * 10 + temp.charAt(j) - 48;
            }
            if(num > Integer.MAX_VALUE){//如果截取的数字大于int的最大值，则停止截取
                break;
            }
            if(res.size() >= 2 && (int)num > res.get(res.size() - 1) + res.get(res.size() - 2)){//注意：这里不能是 !=，因为如果是 < 的话，还有可能成为斐波那契数列.例如：1, 9, 1, 0
                break;
            }
            if(res.size() <= 1 || (int)num == res.get(res.size() - 1) + res.get(res.size() - 2)){
                res.add((int)num);
                if(backtracking(s, i + 1)){
                    return true;
                }
                res.remove(res.size() - 1);
            }
        }
        return false;
    }
}
```