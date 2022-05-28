

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。
对每个孩子 i，都有一个胃口值 g[i]，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 j，都有一个尺寸 s[j] 。如果 s[j] >= g[i]，我们可以将这个饼干 j 分配给孩子 i ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。
示例 :
输入: g = [1,2,3], s = [1,1]
输出: 1
解释: 你有三个孩子和两块小饼干，3个孩子的胃口值分别是：1,2,3。
虽然你有两块小饼干，由于他们的尺寸都是1，你只能让胃口值是1的孩子满足。所以你应该输出1。
思路：
贪心算法：贪心的本质是选择每一阶段的局部最优，从而达到全局最优
这里的局部最优就是大饼干喂给胃口大的，充分利用饼干尺寸喂饱每一个，全局最优就是喂饱尽可能多的小孩

```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        int res = 0;
        Arrays.sort(g);
        Arrays.sort(s);
        int index = s.length - 1;// 饼干数组的下标
        for(int i = g.length - 1; i >= 0; i--){
            if(index >= 0 && s[index] >= g[i]){
                res++;
                index--;
            }
        }
        return res;
    }
}
```
自己写的：
```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        int res = 0;
        Arrays.sort(g);
        Arrays.sort(s);
        for(int i = g.length - 1, j = s.length - 1; i >= 0 && j >= 0; i--, j--){
            while(i >= 0 && s[j] < g[i]){
                i--;
            }
            if(i >= 0){
                res++;
            }
        }
        return res;
    }
}
```