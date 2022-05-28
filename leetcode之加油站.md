

134. 加油站
在一条环路上有 N 个加油站，其中第 i 个加油站有汽油 gas[i] 升。
你有一辆油箱容量无限的的汽车，从第 i 个加油站开往第 i+1 个加油站需要消耗汽油 cost[i] 升。你从其中的一个加油站出发，开始时油箱为空。
如果你可以绕环路行驶一周，则返回出发时加油站的编号，否则返回 -1。
说明: 
如果题目有解，该答案即为唯一答案。
输入数组均为非空数组，且长度相同。
输入数组中的元素均为非负数。
示例 1:
输入: 
gas  = [1,2,3,4,5]
cost = [3,4,5,1,2]
输出: 3
解释:
从 3 号加油站(索引为 3 处)出发，可获得 4 升汽油。此时油箱有 = 0 + 4 = 4 升汽油
开往 4 号加油站，此时油箱有 4 - 1 + 5 = 8 升汽油
开往 0 号加油站，此时油箱有 8 - 2 + 1 = 7 升汽油
开往 1 号加油站，此时油箱有 7 - 3 + 2 = 6 升汽油
开往 2 号加油站，此时油箱有 6 - 4 + 3 = 5 升汽油
开往 3 号加油站，你需要消耗 5 升汽油，正好足够你返回到 3 号加油站。
因此，3 可为起始索引。
示例 2:
输入: 
gas  = [2,3,4]
cost = [3,4,3]
输出: -1
解释:
你不能从 0 号或 1 号加油站出发，因为没有足够的汽油可以让你行驶到下一个加油站。
我们从 2 号加油站出发，可以获得 4 升汽油。 此时油箱有 = 0 + 4 = 4 升汽油
开往 0 号加油站，此时油箱有 4 - 3 + 2 = 3 升汽油
开往 1 号加油站，此时油箱有 3 - 3 + 3 = 3 升汽油
你无法返回 2 号加油站，因为返程需要消耗 4 升汽油，但是你的油箱只有 3 升汽油。
因此，无论怎样，你都不可能绕环路行驶一周。

暴力解法：
自己写的：
```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int len = gas.length;
        int temp = 0;
        for(int i = 0; i < len; i++){
            if(gas[i] < cost[i]){
                continue;
            }else{
                temp = gas[i] - cost[i];
                int j = 0;
                int index = (i + 1) % len;
                while(j < len){
                    temp += gas[index] - cost[index];
                    if(temp < 0){
                        break;
                    }else{
                        index = (index + 1) % len;
                        j++;
                    }
                }
                if(j == len){
                    return i;
                }
            }
        }
        return -1;
    }
}
```
大佬精简版：
```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int len = gas.length;
        for(int i = 0; i < len; i++){
            int temp = gas[i] - cost[i];
            int index = (i + 1) % len;
            while(temp >= 0 && index != i){
                temp += gas[index] - cost[index];
                index = (index + 1) % len;
            }
            if(temp >= 0 && index == i){
                return i;
            }
        }
        return -1;
    }
}
```
贪心算法：
首先如果总油量减去总消耗大于等于0那么一定可以跑完一圈，说明各个站点的加油站的剩油量 rest[i] 相加之和一定是大于等于0的。
每个加油站的剩余量 rest[i] 为 gas[i] - cost[i]。
i 从0开始累加rest[i]，和记为curSum，一旦curSum小于0，说明 [0, i] 区间都不能作为起始位置，起始位置从 i + 1 算起，再从0计算curSum。
![图示](https://img-blog.csdnimg.cn/20201230162018961.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
局部最优：当前累加 rest[i] 的和curSum一旦小于0，起始位置至少要是 i + 1，因为从 i 开始一定不行(加到 rest[i] 的时候curSum变为负数，说明 rest[i] 就是负数，所以 i 不能作为起点)。
全局最优：找到可以跑一圈的起始位置。

```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int len = gas.length;
        int curSum = 0;//记录res[i]的累加和,若能跑完则用来确定起始位置
        int totalSum = 0;//记录总的和,判断能否跑完一圈
        int start = 0;//记录起始位置
        for(int i = 0; i < len; i++){
            curSum += gas[i] - cost[i];
            totalSum += gas[i] - cost[i];
            if(curSum < 0){//当前res[i]的和curSum一旦小于0
                start = i + 1;//起始位置更新为 i + 1
                curSum = 0;//curSum重置为 0
            }
        }
        if(totalSum < 0){//说明怎么走都不可能跑一圈
            return -1;
        }
        return start;
    }
}
```