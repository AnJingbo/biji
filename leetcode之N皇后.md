

51. N 皇后
n 皇后问题研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。
![8皇后](https://img-blog.csdnimg.cn/20201211144833727.png)
上图为 8 皇后问题的一种解法。
给定一个整数 n，返回所有不同的 n 皇后问题的解决方案。
每一种解法包含一个明确的 n 皇后问题的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。
示例：
输入：4
输出：
[
 [".Q..", "...Q", "Q...", "..Q."],//解法1
 ["..Q.", "Q...", "...Q", ".Q.."]//解法2
]
解释: 4 皇后问题存在两个不同的解法。
提示：皇后彼此不能相互攻击，也就是说：任何两个皇后都不能处于同一条横行、纵行或斜线上。

将搜索过程抽象为一棵树：
![图示](https://img-blog.csdnimg.cn/20201211145118164.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
由图可以看出，二维矩阵中矩阵的高就是这棵树的高度，矩阵的宽就是树形机构中每一个结点的宽度
那么我们利用皇后们的约束条件，来回溯搜索这棵树，只要搜索到树的叶子结点，就说明找到了皇后们的合理位置。

```java
class Solution {
    List<List<String>> res = new ArrayList<>();//记录最终结果
    public List<List<String>> solveNQueens(int n) {
        List<char[]> board = new ArrayList<>();
        for(int i = 0; i < n; i++){
            char[] chars = new char[n];
            Arrays.fill(chars, '.');
            board.add(chars);
        }
        backtracking(n, 0, board);
        return res;
    }
    //递归函数参数
    // n 是棋盘的大小，用 row 来记录当前遍历到棋盘的第几层了
    public void backtracking(int n, int row, List<char[]> board){
        //递归终止条件
        //当递归到棋盘最底层的时候(即叶子结点)，就可以收集结果并返回了
        if(row == n){
            List<String> temp = new ArrayList<>();
            for(char[] chars : board){
                temp.add(new String(chars));
            }
            res.add(temp);
            return;
        }
        //单层搜索的逻辑
        //递归深度就是row控制棋盘的行，每一层的for循环的col控制棋盘的列，一行一列就确定了放置皇后的位置
        for(int col = 0; col < n; col++){
            if(isValid(n, row, col, board)){//验证合法就可以放
                board.get(row)[col] = 'Q';//放置皇后
                backtracking(n, row + 1, board);
                board.get(row)[col] = '.';//回溯
            }   
        }
    }
    public boolean isValid(int n, int row, int col, List<char[]> board){
        //不需要对同行进行检查
        //检查列
        for(int i = 0; i < row; i++){
            if(board.get(i)[col] == 'Q'){
                return false;
            }
        }
        //检查 45度角是否有皇后
        for(int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--){
            if(board.get(i)[j] == 'Q'){
                return false;
            }
        }
        //检查 135度角是否有皇后
        for(int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++){
            if(board.get(i)[j] == 'Q'){
                return false;
            }
        }
        return true;
    }
}
```

 