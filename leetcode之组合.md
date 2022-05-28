

77. 组合
给定两个整数 n 和 k，返回 1 ... n 中所有可能的 k 个数的组合。
示例:
输入: n = 4, k = 2
输出:
[[2,4],[3,4],[2,3],[1,2],[1,3],[1,4]]
思路：k为2，需要2层for循环；k为3，需要3层for循环；k为50，就需要50层for循环。因此暴力解法不可行。
回溯法就用递归来解决嵌套层数的问题，每一次的递归中嵌套一个for循环，那么递归就可以用于解决多层嵌套循环的问题了。
因为用大脑模拟回溯搜索的过程很困难，所以需要抽象图形结构来进一步理解(即用树形结构来理解回溯)
![回溯的树形理解](https://img-blog.csdnimg.cn/20201125211715398.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70#pic_center)
每次从集合中选取元素，可选择的范围随着选择的进行而收缩，调整可选择的范围。
由图可以发现：n相当于树的宽度，k相当于树的深度。
途中每次搜索到了叶子结点，我们就找到了一个结果。
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();//存放符合条件的结果的集合
    List<Integer> list = new ArrayList<>();//存放符合条件的单一结果
    public List<List<Integer>> combine(int n, int k) {
        backtracking(n, k, 0);
        return res;
    }
    //回溯函数的参数和返回值
    //startIndex用来记录本层递归中，集合从哪里开始遍历(集合就是[1, ..., n])
    //每次从集合中选取元素，可选择的范围随着选择的进行而收缩，调整可选择的范围，靠的就是startIndex
    public void backtracking(int n, int k, int startIndex){
        //回溯函数的终止条件
        //当list这个集合的大小如果到达k，说明我们找到一个子集大小为k的组合了，此时用res把list保存起来，并终止本层递归
        //list存的就相当于图中根结点到叶子结点的路径
        if(list.size() == k){
            res.add(new ArrayList<>(list));
            //res.add(list);错误！因为list指向的只有一块内存，之后的list改变后，之前已经add进去的也会改变
            return;
        }
        //单层搜索的过程
        //回溯法的搜索过程就是一个树形结构的遍历过程。for循环是用来横向遍历，递归是用来纵向遍历
        for(int i = startIndex; i < n; i++){
            list.add(i + 1);//处理结点
            backtracking(n, k, i + 1);//递归：控制树的纵向遍历，注意下一层搜索要从i+1开始
            list.remove(list.size() - 1);//回溯，撤销处理的结点
        }
    }
}
```
剪枝操作：
```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();//存放符合条件的结果的集合
    List<Integer> list = new ArrayList<>();//存放符合条件的单一结果
    public List<List<Integer>> combine(int n, int k) {
        backtracking(n, k, 0);
        return res;
    }
    public void backtracking(int n, int k, int startIndex){
        if(list.size() == k){
            res.add(new ArrayList<>(list));
            return;
        }
        //可以剪枝优化：如果for循环所选择的起始位置之后的元素已经不足我们需要的元素个数，那么就没必要搜索了
        //比如：n = 4，k = 4，之后从2，3，4开始的list就一定不符合要求
        /*优化过程：1、已经选择的元素个数：list.size()
                   2、还需要的元素个数：k - list.size();
                   3、在集合n中至多要从该起始位置: n - (k - list.size()) + 1 开始遍历*/
        for(int i = startIndex; i < n - (k - list.size()) + 1;i++){
            list.add(i + 1);
            backtracking(n, k, i + 1);
            list.remove(list.size() - 1);
        }
    }
}
```