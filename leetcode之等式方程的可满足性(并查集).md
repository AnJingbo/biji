

990. 等式方程的可满足性

给定一个由表示变量之间关系的字符串方程组成的数组，每个字符串方程 equations[i] 的长度为 4，并采用两种不同的形式之一："a==b" 或 "a!=b"。在这里，a 和 b 是小写字母（不一定不同），表示单字母变量名。

只有当可以将整数分配给变量名，以便满足所有给定的方程时才返回 true，否则返回 false。 

示例 1：

输入：["a==b","b!=a"]
输出：false
解释：如果我们指定，a = 1 且 b = 1，那么可以满足第一个方程，但无法满足第二个方程。没有办法分配变量同时满足这两个方程。

示例 2：
输入：["b==a","a==b"]
输出：true
解释：我们可以指定 a = 1 且 b = 1 以满足满足这两个方程。

示例 3：
输入：["a==b","b==c","a==c"]
输出：true

示例 4：
输入：["a==b","b!=c","c==a"]
输出：false

示例 5：
输入：["c==c","b==d","x!=z"]
输出：true

提示：
1 <= equations.length <= 500
equations[i].length == 4
equations[i][0] 和 equations[i][3] 是小写字母
equations[i][1] 要么是 '='，要么是 '!'
equations[i][2] 是 '='

思路：将equations中的算式根据 == 和 != 分成两部分，先处理 == 算式，使得它们通过相等关系将等式两边的顶点进行合并；然后处理 != 算式，检查不等关系是否破坏了相等关系的连通性。

```java
class Solution {
    int[] parent = new int[26];
    int[] size = new int[26];
    public boolean equationsPossible(String[] equations) {
        for(int i = 0; i < 26; i++){
            parent[i] = i;
            size[i] = 1;
        }
        for(String temp : equations){//先让相等的字母形成连通分量
            if(temp.charAt(1) == '='){
                char x = temp.charAt(0);
                char y = temp.charAt(3);
                union(x - 'a', y - 'a');
            }
        }
        for(String temp : equations){//检查不等关系是否打破了相等关系的连通性
            if(temp.charAt(1) == '!'){
                char x = temp.charAt(0);
                char y = temp.charAt(3);
                //如果相等关系成立，就是逻辑冲突
                if(conneted(x - 'a', y - 'a')){
                    return false;
                }
            }
        }
        return true;
    }
    public int find(int x){
        while(x != parent[x]){
            parent[x] = parent[parent[x]];
            x = parent[x];
        }
        return x;
    }
    public void union(int p, int q){
        int rootP = find(p);
        int rootQ = find(q);
        if(rootP == rootQ){
            return;
        }
        if(size[rootP] > size[rootQ]){
            parent[rootQ] = rootP;
            size[rootP] += size[rootQ];
        }else{
            parent[rootP] = rootQ;
            size[rootQ] += size[rootP];
        }
    }
    public boolean conneted(int p, int q){
        return find(p) == find(q);
    }
}
```