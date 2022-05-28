

56. 合并区间
给出一个区间的集合，请合并所有重叠的区间。
示例 1:
输入: intervals = [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
示例 2:
输入: intervals = [[1,4],[4,5]]
输出: [[1,5]]
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。
提示：
intervals[i][0] <= intervals[i][1] 

思路：
按照左边界从小到大排序之后：
局部最优：每次合并都取最大的右边界，这样就可以合并更多的区间了。
全局最优：合并所有重叠的区间。
如何模拟合并：其实就是用合并区间后的左边界和右边界作为一个新的区间，加入到res集合中即可。如果没有合并就把原区间加入到res集合

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if(intervals.length == 0){
            return new int[0][2];
        }
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        List<int[]> res = new ArrayList<>();
        res.add(intervals[0]);
        for(int i = 1; i < intervals.length; i++){
            if(intervals[i][0] > intervals[i - 1][1]){//如果不重叠
                res.add(intervals[i]);
                continue;
            }else{
                intervals[i][1] = Math.max(intervals[i][1], intervals[i - 1][1]);
                res.get(res.size() - 1)[1] = Math.max(intervals[i][1], intervals[i - 1][1]);
            }
        }
        return res.toArray(new int[res.size()][2]);
    }
}
```
```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if(intervals.length == 0){
            return new int[0][2];
        }
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        List<int[]> res = new ArrayList<>();
        res.add(intervals[0]);
        for(int i = 1; i < intervals.length; i++){
            int[] cur = intervals[i];
            int[] peek = res.get(res.size() - 1);
            if(cur[0] > peek[1]){//如果不重叠
                res.add(cur);
                continue;
            }else{
                peek[1] = Math.max(cur[1], peek[1]);
            }
        }
        return res.toArray(new int[res.size()][2]);
    }
}
```