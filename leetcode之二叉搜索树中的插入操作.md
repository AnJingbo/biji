

```java
/*
701. 二叉搜索树中的插入操作
给定二叉搜索树（BST）的根节点和要插入树中的值，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 输入数据 保证 ，新值和原始二叉搜索树中的任意节点值都不同。
注意，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回 任意有效的结果 。
示例 ：
输入：root = [40,20,60,10,30,50,70], val = 25
输出：[40,20,60,10,30,50,70,null,null,25]
*/
//递归
//有返回值的写法
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if(root == null){
            return new TreeNode(val);
        }
        //利用递归函数返回值，完成新加入结点的父子关系的赋值操作
        //下一层将加入结点返回，本层用root.left或者root.right将其接住
        if(val < root.val){
            root.left = insertIntoBST(root.left, val);
        }
        if(val > root.val){
            root.right = insertIntoBST(root.right, val);
        }
        return root;
    }
}
//没有返回值的写法
//找到插入的结点位置，直接让其父结点指向插入结点，结束递归
class Solution {
    TreeNode pre = null;//记录前一个结点
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if(root == null){
            return new TreeNode(val);
        }
        traversal(root, val);
        return root;
    }
    public void traversal(TreeNode root, int val){
        if(root == null){
            TreeNode node = new TreeNode(val);
            if(val < pre.val){
                pre.left = node;
            }else{
                pre.right = node;
            }
            return;
        }
        pre = root;
        if(val < root.val){
            traversal(root.left, val);
        }
        if(val > root.val){
            traversal(root.right, val);
        }
    }
}
//迭代
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if(root == null){
            return new TreeNode(val);
        }
        TreeNode temp = root;
        while(temp != null){
            if(val < temp.val){
                if(temp.left == null){
                    TreeNode node = new TreeNode(val);
                    temp.left = node;
                    break;
                }
                temp = temp.left;
            }else if(val > temp.val){
                if(temp.right == null){
                    TreeNode node = new TreeNode(val);
                    temp.right = node;
                    break;
                }
                temp = temp.right;
            }
        }
        return root;
    }
}
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if(root == null){
            return new TreeNode(val);
        }
        TreeNode cur = root;
        TreeNode pre = root;//记录上一个结点，否则无法赋值新结点
        while(cur != null){
            pre = cur;
            if(val < cur.val){
                cur = cur.left;
            }else{
                cur = cur.right;
            }
        }
        TreeNode node = new TreeNode(val);
            if(val < pre.val){
                pre.left = node;
            }else{
                pre.right = node;
            }
        return root;
    }
}
```