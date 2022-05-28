

最小生成树能够保证整个拓扑图的**所有路径之和最小**，但不能保证任意两点之间是最短路径。
![无向图](https://img-blog.csdnimg.cn/20201122204114801.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70#pic_center)
普里姆(Prim)算法(加点法):
(1) 从A顶点开始处理: 得到 < A >
(2) < A >开始，将和 A 顶点相邻并且还没有被访问的顶点进行处理:
A-C[7]; A-G[2]; A-B[5]; 得到 <A, G>
(3) <A, G>开始，将和 A, G 顶点相邻并且还没有被访问过的顶点进行处理：
A-C[7]; A-B[5]; G-B[3]; G-E[4]; G-F[6]; 得到 <A, G, B>
(4) <A, G, B>开始，将和 A, G, B 顶点相邻并且还没有被访问过的顶点进行处理:  A-C[7]; G-E[4]; G-F[6]; B-D[9]; 得到 <A, G, B, E>
(5) <A, G, B, E>开始，将和 A, G, B, E 顶点相邻并且还没有被访问过的顶点进行处理：A-C[7]; G-F[6]; B-D[9]; E-C[8]; E-F[5]; 得到 <A, G, B, E, F>
(6) <A, G, B, E, F>开始，将和 A, G, B, E, F 顶点相邻并且还没有被访问过的顶点进行处理：A-C[7]; B-D[9]; E-C[8]; F-D[4]; 得到 <A, G, B, E, F, D>
(7)<A, G, B, E, F, D>开始，将和 A, G, B, E, F, D 顶点相邻并且还没有被访问过的顶点进行处理：A-C[7]; E-C[8];  得到 <A, G, B, E, F, D, C>
一共会生成6条边: A-G[2]; G-B[3]; G-E[4]; E-F[5]; F-D[4]; A-C[7]
（n个顶点的最小二叉树一共有n - 1条边）
代码实现：

```java
public class Prim {
    public static void main(String[] args) {
        MGraph graph = new MGraph(7);
        MinTree minTree = new MinTree();
        char[] data = {'A', 'B', 'C', 'D', 'E', 'F', 'G'};
        final int N = 10000;//用10000表示两个结点之间不能连接，或者也可以用Integer.MAX_VALUE
        int[][] weight = {
                {N, 5, 7, N, N, N, 2},
                {5, N, N, 9, N, N, 3},
                {7, N, N, N, 8, N, N},
                {N, 9, N, N, N, 4, N},
                {N, N, 8, N, N, 5, 4},
                {N, N, N, 4, 5, N, 6},
                {2, 3, N, N, 4, 6, N}};
        minTree.creatGraph(graph, 7, data, weight);
        minTree.show(graph);
        minTree.prim(graph, 0);
    }
}

class MinTree {
    public void creatGraph(MGraph graph, int vertex, char[] data, int[][] weight) {
        for (int i = 0; i < vertex; i++) {
            graph.data[i] = data[i];
            for (int j = 0; j < vertex; j++) {
                graph.weight[i][j] = weight[i][j];
            }
        }
    }

    public void show(MGraph graph) {
        for (int i = 0; i < graph.vertex; i++) {
            for (int j = 0; j < graph.vertex; j++) {
                System.out.print(graph.weight[i][j] + "  ");
            }
            System.out.println();
        }
    }

    /**
     * @param graph 将图传进去
     * @param v 指从下标为 v 的结点开始
     */
    public void prim(MGraph graph, int v) {
        boolean[] isVisited = new boolean[graph.vertex];//false表示未被访问过，true表示已经被访问过
        //把当前结点标记为已访问
        isVisited[v] = true;
        //minWeight记录最小权的那个值
        int minWeight = 10000;
        //h1和h2记录两个顶点的下标
        int h1 = -1;
        int h2 = -1;
        for (int k = 1; k < graph.vertex; k++) {//因为有n个结点，在算法结束后，一共会有 n - 1 个边
            //确定每一次生成的子图，和哪个结点的距离最近(权值最小)
            for (int i = 0; i < graph.vertex; i++) { //i表示已经访问过的结点
                for (int j = 0; j < graph.vertex; j++) { //j表示还未被访问过的结点
                    //从已经访问过的结点中找到与它们相邻的、还未被访问过的并且权值最小的那个顶点，其下标即 h2
                    if (isVisited[i] == true && isVisited[j] == false && graph.weight[i][j] < minWeight) {
                    //如果i结点被访问过并且j结点未被访问并且当前权值小于最小权值
                        minWeight = graph.weight[i][j];
                        h1 = i;
                        h2 = j;
                    }
                }
            }
            //将这个结点标记为已经访问
            isVisited[h2] = true;
            System.out.println("边<" + graph.data[h1] + ", " + graph.data[h2] + ">, 其权值为：" + graph.weight[h1][h2]);
            //minWeight 重新赋值为最大值10000
            minWeight = 10000;
        }
    }
}

class MGraph {
    int vertex;//记录结点的个数
    char[] data;//记录各个结点的值
    int[][] weight;//记录邻接矩阵

    public MGraph(int n) {
        vertex = n;
        data = new char[n];
        weight = new int[n][n];
    }
}
```