

```java
/*
700. 二叉搜索树中的搜索
给定二叉搜索树（BST）的根节点和一个值。 你需要在BST中找到节点值等于给定值的节点。 返回以该节点为根的子树。 如果节点不存在，则返回 NULL。
例如，
给定二叉搜索树:
        4
       / \
      2   7
     / \
    1   3
和值: 2
你应该返回如下子树:
      2     
     / \   
    1   3
在上述示例中，如果要找的值是 5，但因为没有节点值为 5，我们应该返回 NULL。
*/
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
         if(root == null){
            return null;
        }
        if(root.val == val){
            return root;
        }
        if(val < root.val){
            return searchBST(root.left, val);
        }
        if(val > root.val){
            return searchBST(root.right, val);
        }
        return null;
    }
}
//因为二叉搜索树的有序性，可以不使用辅助栈或者队列就可以写出迭代法
//对于一般二叉树，递归过程中还有回溯的过程，例如走一个左方向的分支走到头了，那么还要掉头，再走右分支。
//但对于二叉搜索树，不需要回溯的过程，因为结点的有序性就帮我们确定了搜索的方向
//非递归(迭代法)
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if(root == null){
            return null;
        }
        while(root != null){
            if(root.val == val){
                return root;
            }else if(val < root.val){
                root = root.left;
            }else{
                root = root.right;
            }
        }
        return null;
    }
}
```