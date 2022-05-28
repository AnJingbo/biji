

最小生成树能够保证整个拓扑图的**所有路径之和最小**，但不能保证任意两点之间是最短路径。
![示例图](https://img-blog.csdnimg.cn/20201124153227934.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70#pic_center)
克鲁斯卡尔(Kruskal)算法(加边法)：
基本思想：按照权值从大到小的顺序选择 n - 1 条边，并保证这 n - 1 条边不构成回路
克鲁斯卡尔算法需重点解决两个问题：
1、对图的所有边按照权值的大小进行排序
解决：采用排序算法排序即可
2、将边添加到最小生成树中时，怎样判断是否形成了回路
解决：加入的边的两个顶点不能都指向同一个终点，否则会形成回路(终点定义：将所有顶点按照从小到大的顺序排列好之后，某个顶点的终点就是与它连通的最大顶点)
比如：在第三步后面不能加入<C, E>这条边，因为C、E此时都指向F这个终点；也不能加入<C, F>这条边，因为C、F此时也都指向F这个终点；而可以加入<B, F>这条边，是因为此时F的终点是F，而B此时还没有终点，因此可以加入这条边
第一步：选取边<E, F>;
第二步：选取边<C, D>;
第三步：选取边<E, D>;
第四步：选取边<B, F>;
第五步：选取边<G, E>;
第六步：选取边<A, B>;
（n个顶点的最小二叉树一共有n - 1条边）
判断是否有环：
克鲁斯卡尔算法实质上是使生成树以一种随意的方式生长，初始时每个顶点构成一棵生成树，然后每生长一次就将两棵树合并，到最后合并成一棵树。
因此，可以设置一个数组parent[n]，元素parent[i]表示顶点 i 的双亲结点，初始时，parent[i] = -1, 表示顶点 i 没有双亲，即该结点是所在生成树的根结点；对于边<u, v>，设 vertex1 和 vertex2 分别表示两个顶点所在树的根结点，如果 vertex1 不等于 vertex2，则顶点v和u必定位于不同的连通分量，令 parent[vertex2] = vertex1，实现两棵树的合并。

代码实现：

```java
public class Kruskal {
    public static void main(String[] args) {
        char[] vertexs = {'A', 'B', 'C', 'D', 'E', 'F', 'G'};
        final int N = 10000;//用10000表示两个结点之间不能连接，或者也可以用Integer.MAX_VALUE
        int[][] weight = {
                {N, 12, N, N, N, 16, 14},
                {12, N, 10, N, N, 7, N},
                {N, 10, N, 3, 5, 6, N},
                {N, N, 3, N, 4, N, N},
                {N, N, 5, 4, N, 2, 8},
                {16, 7, 6, N, 2, N, 9},
                {14, N, N, N, 8, 9, N}};
        MGraph graph = new MGraph(vertexs, weight);
        Edge[] edges = graph.getEdges();
        graph.show(edges);
        System.out.println("排序后：");
        graph.sort(edges);
        graph.show(edges);
        System.out.println("最小生成树为：");
        graph.kruskal(edges);
    }
}

class MGraph {
    char[] vertexs;//记录各个结点的值
    int[][] weight;//记录邻接矩阵
    int numOfVertexs;//记录结点的个数
    int numOfEdges = 0;//记录边的个数
    public MGraph(char[] vertexs, int[][] weight) {
        this.vertexs = vertexs;
        this.weight = weight;
        this.numOfVertexs = vertexs.length;
        for(int i = 0; i < numOfVertexs; i++){
            for(int j = i + 1; j < numOfVertexs; j++){//注意是 j = i + 1，因为无向图是对称的，只需要记录一次边的个数即可
                if(weight[i][j] != 10000){
                    numOfEdges++;
                }
            }
        }
    }

    public Edge[] getEdges(){
        Edge[] edges = new Edge[numOfEdges];
        int index = 0;
        for(int i = 0; i < numOfVertexs; i++){
            for(int j = i + 1; j < numOfVertexs; j++){//注意是 j = i + 1，因为无向图是对称的，只需要记录一次边的个数即可
                if(weight[i][j] != 10000){
                    edges[index++] = new Edge(vertexs[i], vertexs[j], weight[i][j]);
                }
            }
        }
        return edges;
    }

    public void sort(Edge[] edges){
        for(int i = 0; i < edges.length - 1; i++){
            for(int j = 0; j < edges.length - i - 1; j++){
                if(edges[j].weight > edges[j + 1].weight){
                    Edge temp = edges[j];
                    edges[j] = edges[j + 1];
                    edges[j + 1] = temp;
                }
            }
        }
    }

    public void kruskal(Edge[] edges){
    //元素parent[i]表示顶点 i 的双亲结点
    //初始时，parent[i] = -1, 表示顶点 i 没有双亲，即该结点是所在生成树的根结点
        int[] parent = new int[numOfVertexs];
        for(int i = 0; i < numOfVertexs; i++){
            parent[i] = -1;
        }
        for(int i = 0, num = 0; i < numOfEdges; i++){
            int vertex1 = findRoot(parent, edges[i].from);//得到所在生成树的根结点
            int vertex2 = findRoot(parent, edges[i].to);//得到所在生成树的根结点
            if(vertex1 != vertex2){//找到两个根结点不同，则不会构成环
                System.out.println(edges[i]);
                parent[vertex2] = vertex1;//合并生成树
                num++;
                if(num == numOfVertexs - 1){//循环了"图的顶点数 - 1"次，结束循环
                    return;
                }
            }
        }
    }

    public int findRoot(int[] parent, char v){
        int temp = getIndexOfVertex(v);
        while(parent[temp] != -1){
            temp = parent[temp];
        }
        return temp;
    }

    public int getIndexOfVertex(char c){//得到某个顶点对应的下标
        for(int i = 0; i < numOfVertexs; i++){
            if(vertexs[i] == c){
                return i;
            }
        }
        return -1;
    }

    public void show(Edge[] edges){
        for(int i = 0; i < edges.length; i++){
            System.out.println(edges[i]);
        }
    }
}
class Edge{
    char from;//边的起始顶点
    char to;//边的终止顶点
    int weight;//边的权值
    public Edge(char from, char to, int weight){
        this.from = from;
        this.to = to;
        this.weight = weight;
    }
    @Override
    public String toString() {
        return "Edge{" + "from=" + from + ", to=" + to + ", weight=" + weight + '}';
    }
}
```