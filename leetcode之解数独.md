

37 解数独
编写一个程序，通过填充空格来解决数独问题。
一个数独的解法需遵循如下规则：
数字 1-9 在每一行只能出现一次。
数字 1-9 在每一列只能出现一次。
数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。
空白格用 '.' 表示。
![图示1](https://img-blog.csdnimg.cn/20201213150707158.png)
![图示2](https://img-blog.csdnimg.cn/20201213150716517.png)
答案被标成红色。
提示：
给定的数独序列只包含数字 1-9 和字符 '.' 。
你可以假设给定的数独只有唯一解。
给定数独永远是 9x9 形式的。

部分树形结构：
![图示2](https://img-blog.csdnimg.cn/20201213150810171.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)

```java
class Solution {
    public void solveSudoku(char[][] board) {
        backtracking(board);
    }
    //递归函数以及参数
    //解数独找到一个符合的条件(就在树的叶子结点上)就立刻返回，相当于找从根结点到叶子结点的一条唯一路径，所以需要返回值
    public boolean backtracking(char[][] board){
        //递归终止条件
        //本题递归不用终止条件。递归的下一层棋盘一定比上一层棋盘多一个数，等装填满了棋盘自然就终止，所以不需要终止条件
        //单层逻辑
        //一个for循环遍历棋盘的行，一个for循环遍历棋盘的列
        //一行一列确定下来之后，递归遍历这个位置放9个数的可能
        for(int i = 0; i < 9; i++){//遍历行
            for(int j = 0; j < 9; j++){//遍历列
                if(board[i][j] != '.'){
                    continue;
                }
                for(char k = '1'; k <= '9'; k++){// (i, j)这个位置放 k 是否合适
                    if(isValid(board, i, j, k)){//判断是否合法
                        board[i][j] = k;//放置 k
                        if(backtracking(board)){//如果找到合适的一组，立刻返回
                            return true;
                        }
                        board[i][j] = '.';//回溯
                    }
                }
                return false;// 9个数都试完了，都不行，就返回false(这也是为什么没有终止条件也不会永远填不满棋盘的原因)
            }
        }
        return true;//遍历完没有返回false，说明找到了合适的棋盘位置了
    }

    public boolean isValid(char[][] board, int row, int col, int val){
        for(int i = 0; i < 9; i++){
            if(board[row][i] == val){
                return false;
            }
        }
        for(int i = 0; i < 9; i++){
            if(board[i][col] == val){
                return false;
            }
        }
        int startRow = (row / 3) * 3;
        int startCol = (col / 3) * 3;
        for(int i = startRow; i < startRow + 3; i++){
            for(int j = startCol; j < startCol + 3; j++){
                if(board[i][j] == val){
                    return false;
                }
            }
        }
        return true;
    }
}
```