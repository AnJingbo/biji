

### 1329. 将矩阵按对角线排序
![ts](https://img-blog.csdnimg.cn/f02da2e7780541bab2563139ca4949e5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)

```java
/**
假设元素的坐标为 (i,j)，则同一对角线上的元素的 j-i 的值相等。比如：
3 3 1 1    (0,0) (0,1) (0,2) (0,3)
2 2 1 2    (1,0) (1,1) (1,2) (1,3)
1 1 1 2    (2,0) (2,1) (2,2) (2,3)
对于 (2,0) 这条对角线，其 j-i 的值为 -2；对于 (1,0)(2,1) 这条对角线，其 j-i 的值为 -1；
对于 (0,0)(1,1)(2,2) 这条对角线，其 j-i 的值为 0；对于 (0,1)(1,2)(2,3) 这条对角线，其 j-i 的值为 1；
对于 (0,2)(1,3) 这条对角线，其 j-i 的值为 2；对于 (0,3) 这条对角线，其 j-i 的值为 3
 */
class Solution {
    // 假设元素的坐标为(i,j)，则同一对角线上的元素的 j-i 的值相等
    public int[][] diagonalSort(int[][] mat) {
        int m = mat.length, n = mat[0].length;
        int len = m + n - 1; // 对角线的条数
        // 每一条对角线都创建一个动态数组
        ArrayList<Integer>[] lists = new ArrayList[len];
        for(int i = 0; i < len; i++){
            lists[i] = new ArrayList<>();
        }
        // 遍历原始矩阵，把原始矩阵中的元素放进对应的动态数组中
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                // 得到坐标 (i,j) 对应的下标
                int index = j - i + (m - 1);
                lists[index].add(mat[i][j]);
            }
        }
        for(int i = 0; i < len; i++){
            Collections.sort(lists[i]);
        }
        int[] next = new int[len];
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                int index = j - i + (m - 1);
                mat[i][j] = lists[index].get(next[index]++);
            }
        }
        return mat;
    }
}
```
```java
class Solution {
    public int[][] diagonalSort(int[][] mat) {
        int m = mat.length, n = mat[0].length;
        for(int j = 0; j < n; j++){
            List<Integer> list = new ArrayList<>();
            list.add(mat[0][j]);
            int row = 1, col = j + 1;
            while(row < m && col < n){
                list.add(mat[row++][col++]);
            }
            Collections.sort(list);
            int index = 0;
            row = 0;
            col = j;
            while(row < m && col < n){
                mat[row++][col++] = list.get(index++);
            }
        }
        for(int i = 1; i < m; i++){
            List<Integer> list = new ArrayList<>();
            list.add(mat[i][0]);
            int row = i + 1, col = 1;
            while(row < m && col < n){
                list.add(mat[row++][col++]);
            }
            Collections.sort(list);
            int index = 0;
            row = i;
            col = 0;
            while(row < m && col < n){
                mat[row++][col++] = list.get(index++);
            }
        }
        return mat;
    }
}
```