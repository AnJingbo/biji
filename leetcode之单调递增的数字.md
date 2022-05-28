

738. 单调递增的数字
给定一个非负整数 N，找出小于或等于 N 的最大的整数，同时这个整数需要满足其各个位数上的数字是单调递增。
当且仅当每个相邻位数上的数字 x 和 y 满足 x <= y 时，我们称这个整数是单调递增的。）
示例 1:
输入: N = 10
输出: 9
示例 2:
输入: N = 1234
输出: 1234
示例 3:
输入: N = 332
输出: 299


思路：
从前到后找到第一个满足 strNum[i - 1] > strNum[i] 的位置，然后把 strNum[i - 1] - 1，再把后面的位置都变成 9 即可。比如：
n   = 1234321
res = 1233999
但是由于减小了strNum[i] 以后，可能不满足 strNum[i-1] <= strNum[i] 了，所以我们需要从后向前遍历。比如：
n    = 2333332
res  = 2299999

题目要求小于等于N的最大单调递增的整数，那么拿一个两位数来举例：
比如：98，一旦出现strNum[i - 1] > strNum[i]的情况（即非单调递增），首先想让 strNum[i - 1] --，然后strNum[i]给为9，这样这个整数就是89，即89就是小于98的最大的递增整数。
局部最优：遇到strNum[i - 1]  > strNum[i]，让strNum[i - 1]--，然后strNum[i] 给为9，可以保证这两位变成最大单调递增整数。
全局最优：得到小于等于N的最大单调递增的整数。
但这里局部最优推出全局最优，还需要其他条件，即遍历顺序和标记从哪一位开始统一变为9。
如果从前向后遍历的话，遇到strNum[i - 1] > strNum[i]的情况，让strNum[i - 1]减一，但此时如果strNum[i - 1]减一了，可能又小于strNum[i - 2]。
比如：332，从前向后遍历的话，那么结果就变成了329，但此时2又小于了第一位的3了，真正的结果应该是299。
所以要从后向前遍历，就可以重复利用上次比较得出的结果了。从后向前遍历332的数值变化为：332 -> 329 -> 299。

```java
class Solution {
    public int monotoneIncreasingDigits(int N) {
        String str = "" + N;
        char[] strNum = str.toCharArray();
        // flag 用来标记赋值 9 从哪里开始
        //设置这个默认值，是为了防止第二个for循环在flag没有被赋值的情况下进行
        int flag = strNum.length;
        for(int i = strNum.length - 1; i > 0; i--){
            if(strNum[i - 1] > strNum[i]){
                flag = i;
                strNum[i - 1]--;
            }
        }
        for(int i = flag; i < strNum.length; i++){
            strNum[i] = '9';
        }
        return Integer.parseInt(new String(strNum));
    }
}
```