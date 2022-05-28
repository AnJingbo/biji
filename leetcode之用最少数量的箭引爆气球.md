

在二维空间中有许多球形的气球。对于每个气球，提供的输入是水平方向上，气球直径的开始和结束坐标。由于它是水平的，所以纵坐标并不重要，因此只要知道开始和结束的横坐标就足够了。开始坐标总是小于结束坐标。
一支弓箭可以沿着 x 轴从不同点完全垂直地射出。在坐标 x 处射出一支箭，若有一个气球的直径的开始和结束坐标为 xstart，xend， 且满足  xstart ≤ x ≤ xend，则该气球会被引爆。可以射出的弓箭的数量没有限制。 弓箭一旦被射出之后，可以无限地前进。我们想找到使得所有气球全部被引爆，所需的弓箭的最小数量。
给你一个数组 points ，其中 points [i] = [xstart,xend] ，返回引爆所有气球所必须射出的最小弓箭数。

示例 1：
输入：points = [[10,16],[2,8],[1,6],[7,12]]
输出：2
解释：对于该样例，x = 6 可以射爆 [2,8],[1,6] 两个气球，以及 x = 11 射爆另外两个气球
示例 2：
输入：points = [[1,2],[3,4],[5,6],[7,8]]
输出：4
示例 3：
输入：points = [[2,3],[2,3]]
输出：1

思路：
局部最优：当气球出现重叠，一起射，所用弓箭最少。
全局最优：把所有气球射爆所用弓箭最少。
为了让气球尽可能地重叠，需要对数组进行排序。
排序之后，如果气球不重叠，则再需要一支箭；如果气球重叠了，重叠气球中右边界的最小值之前的区间一定需要一支弓箭。![图示](https://img-blog.csdnimg.cn/20210102173238184.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
由图可以看出，第一组重叠气球，一定是需要一支箭；气球 3 的左边界大于了第一组重叠气球的最小右边界，所以再需要一支箭来射气球 3。

代码实现：
```java
class Solution {
    public int findMinArrowShots(int[][] points) {
        if(points.length == 0){
            return 0;
        }
        Arrays.sort(points, new Comparator<int[]>(){
            public int compare(int[] a, int[] b){
                return a[0] > b[0] ? 1 : -1;
                //return a[0] - b[0];注意：本题中不能写成这样，因为有可能会超出 int 的取值范围
                //比如[[-2147483646,-2147483645],[2147483646,2147483647]]
            }
        });
        int res = 1;//points 不为空时至少需要一支箭
        for(int i = 1; i < points.length; i++){
            if(points[i][0] > points[i - 1][1]){//气球 i 和气球 i - 1 不挨着，注意这里不是 >=
                res++;//需要一支箭
            }else{//气球 i 和气球 i - 1 挨着
                points[i][1] = Math.min(points[i - 1][1], points[i][1]);
            }
        }
        return res;
    }
}
```