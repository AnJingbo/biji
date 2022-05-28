

```java
public class LianTongXing {
    public static void main(String[] args) {
        // 0代表无元素，1代表有元素
        int[][] map = {
                {1, 1, 1, 0},
                {0, 0, 0, 1},
                {0, 0, 0, 1}
        };

        int ans = 0; // 记录连通块的数目
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 4; j++) {
                if (map[i][j] == 1) {
                    dfs(map, i, j);
                    ans++;
                }
            }
        }
        System.out.println(ans);
    }

    public static void dfs(int[][] map, int i, int j) {
        map[i][j] = 0;
        int m = map.length, n = map[0].length; // m行n列
        if (i - 1 >= 0 && map[i - 1][j] == 1) {
            dfs(map, i - 1, j);
        }
        if (i + 1 < m && map[i + 1][j] == 1) {
            dfs(map, i + 1, j);
        }
        if (j - 1 >= 0 && map[i][j - 1] == 1) {
            dfs(map, i, j - 1);
        }
        if (j + 1 < n && map[i][j + 1] == 1) {
            dfs(map, i, j + 1);
        }
    }
}
```
```java
public class LianTongXing {
    public static void main(String[] args) {
        // 0 代表无元素，1 代表有元素
        int[][] map = {
                {1, 1, 1, 0},
                {0, 0, 0, 1},
                {0, 0, 0, 1}
        };

        int ans = 0; // 记录连通块的数目
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 4; j++) {
                if (map[i][j] == 1) {
                    dfs(map, i, j);
                    ans++;
                }
            }
        }
        System.out.println(ans);
    }

    public static void dfs(int[][] map, int i, int j) {
        int m = map.length, n = map[0].length; // m行n列
        if(i < 0 || i >= m || j < 0 || j >= n || map[i][j] != 1){
            return;
        }
        map[i][j] = 0;
        dfs(map, i - 1, j);
        dfs(map, i + 1, j);
        dfs(map, i, j - 1);
        dfs(map, i, j + 1);
    }
}
```