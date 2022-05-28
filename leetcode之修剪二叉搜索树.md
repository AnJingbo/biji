

```java
/*
669. 修剪二叉搜索树
给你二叉搜索树的根节点 root ，同时给定最小边界low 和最大边界 high。通过修剪二叉搜索树，使得所有节点的值在[low, high]中。修剪树不应该改变保留在树中的元素的相对结构（即，如果没有被移除，原有的父代子代关系都应当保留）。 可以证明，存在唯一的答案。

所以结果应当返回修剪好的二叉搜索树的新的根节点。注意，根节点可能会根据给定的边界发生改变。
示例：
     输入：root = [3,0,4,null,2,null,null,1], low = 1, high = 3
     输出：[3,2,null,1]
*/
class Solution {
    //确定递归函数的参数和返回值
    public TreeNode trimBST(TreeNode root, int low, int high) {
        //确定终止条件
        if(root == null){
            return null;
        }
        //确定单层递归的逻辑
        //如果当前结点的元素小于low的值，说明当前结点的左子树需要砍掉，那么应该递归右子树，并返回右子树符合条件的头结点
        if(root.val < low){
            TreeNode right = trimBST(root.right, low, high);
            return right;
        }
        //如果当前结点的元素大于high，说明当前结点的右子树需要砍掉，那么应该递归左子树，并返回左子树符合条件的头结点
        if(root.val > high){
            TreeNode left= trimBST(root.left, low, high);
            return left;
        }
        //将下一层处理完左子树的结果赋给root.left，处理完右子树的结果赋值给root.right
        root.left = trimBST(root.left, low, high);
        root.right = trimBST(root.right, low, high);
        return root;
    }
}
```