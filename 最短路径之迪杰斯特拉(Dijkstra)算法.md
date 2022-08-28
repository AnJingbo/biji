

>最短路径：
>(1) 在网图中，指两顶点之间经历的边上权值之和最短的路径
>(2) 在非网图中，指两顶点之间经历的边数最少的路径
>单源最短路径问题：(源点就是起点，即起点已经确定且只有一个)
>问题描述：给定带权有向图 G=<V, E> 和源点 v ∈V，求从源点 v 到G中其余各顶点的最短路径。
>迪杰斯特拉(Dijkstra)算法 ：按路径长度递增的次序产生最短路径
>迪杰斯特拉算法能够解决**边权重非负**的加权有向图的单起点最短路径问题。

![图示](https://img-blog.csdnimg.cn/20201203151612805.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)

求从**A**到**各顶点**的**最短距离**：

初始：S = { A }

第一步：S = { A, B }
过程：A->B: (A, B) 10;     A->C: (A, C) ∞;    A->D: (A, D) 30;    A->E: (A, E) 100

第二步：S = { A, B, D }
过程：A->B: (A, B) 10;    A->C: (A, B, C) 60;    A->D: (A, D) 30;    A->E: (A, E) 100

第三步：S = { A, B, D, C }
过程：A->B: (A, B) 10;    A->C: (A, D, C) 50;    A->D: (A, D) 30;    A->E: (A, D, E) 90

第四步：S = { A, B, D, C, E }
过程：A->B: (A, B) 10;    A->C: (A, D, C) 50;    A->D: (A, D) 30;    A->E: (A, D, C, E) 60

```java
public class Dijkstra {
    // N可以全部初始化为最大值，但一定要判断是否为最大值以避免运算时溢出
    static final int N = 10000;//表示不可连接
    public static void main(String[] args) {
        int[][] weight = {
                {N, 10, N, 30, 100},
                {N, N, 50, N, N},
                {N, N, N, N, 10},
                {N, N, 20, N, 60},
                {N, N, N, N, N},
        };
        //求从 0 到各顶点的最短距离
        dijkstra(weight, 0);
    }
    
    // 给定一幅图和一个起点(源点) source，计算 source 到其它结点的最短路径的长度 
    public static void dijkstra(int[][] weight, int source){
        // 记录最短路径的长度。shortest[i] 表示源点 source 到结点 i 的最短路径的长度
        int[] shortest = new int[weight.length];
        // 记录最短路径是否已经被求出。isVisited[i] 表示源点 source 到结点 i 的最短路径的长度是否已经被求出
        boolean[] isVisited = new boolean[weight.length];
        // 记录从源点到结点 i 的输出路径
        String[] path = new String[weight.length];

        // 初始化输出路径
        for(int i = 0; i < path.length; i++){
            path[i] = new String(source + "->" + i);
        }
        // 初始化源节点
        shortest[source] = 0;
        isVisited[source] = true;

        for(int i = 1; i < weight.length; i++){
            int min = Integer.MAX_VALUE;
            int index = -1;

            // 找到和源点相连的权值较小的点
            for(int j = 0; j < weight.length; j++){
                // 已经求出最短路径的结点不需要再加入计算 并且 判断从源点到其他节点是否存在更短路径(j一直在变化，就是在遍历其他节点)
                if(isVisited[j] == false && weight[source][j] < min){
                    min = weight[source][j];
                    index = j;
                }
            }
            // 更新最短路径
            shortest[index] = min;
            isVisited[index] = true;

            // 更新源点借助 index 这个点到其他节点的较短路径
            for(int j = 0; j < weight.length; j++){
                if(isVisited[j] == false && weight[source][index] + weight[index][j] < weight[source][j]){
                    weight[source][j] = weight[source][index] + weight[index][j];
                    path[j] = path[index] + "->" + j;
                }
            }
        }
        // 打印最短路径
        for(int i = 0; i < shortest.length; i++){
            if(i != source){
                if(shortest[i] == N){
                    System.out.println(source + "到" + i + "不可达");
                }else{
                    System.out.println(source + "到" + i + "的最短路径为" + path[i] + ", 最短距离是" + shortest[i]);
                }
            }
        }
    }
}
```
>Floyd算法与Dijkstra算法的不同
>1、**Floyd算法是求任意两点之间的最短路径，是多源最短路径。而Dijkstra(迪杰斯特拉)算法是求某一个顶点到其他所有顶点的最短路径，是单源最短路径。**
>2、Floyd算法属于**动态规划**，我们在写核心代码时候就是相当于推dp状态方程，Dijkstra(迪杰斯特拉)算法则属于**贪心算法**。
>3、Dijkstra(迪杰斯特拉)算法时间复杂度一般是**o($n^2$)**，Floyd算法时间复杂度是**o($n^3$)**，Dijkstra(迪杰斯特拉)算法比Floyd算法块。
>4、Floyd算法可以计算**带负权值**的；而Dijkstra算法只能解决**边权重非负**的最短路径问题。
>5、Dijkstra算法通过选定的被访问节点，**求出从源节点到其他节点的最短路径**；而Floyd算法中每一个节点都是出发节点(源节点)，所以需要将每一个节点看作被访问节点，**求出从每一个节点到其他节点的最短路径**
>比如：Dijkstra算法一次只能求取A到B、C的最短路径长度，即它是单源最短路径。而Floyd算法只需要一次就可以求出 A到B、C，B到A、C，C到A、B 所有的最短路径，即它是多源最短路径。
