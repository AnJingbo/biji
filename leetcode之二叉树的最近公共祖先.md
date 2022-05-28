

```java
/*
236. 二叉树的最近公共祖先
给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。
最近公共祖先：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”
例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]
示例 1:
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
示例 2:
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出: 5
解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
说明:
     所有节点的值都是唯一的。
     p、q 为不同节点且均存在于给定的二叉树中。
*/
class Solution {
    //思路：如果找到一个结点，发现左子树出现结点p，右子树出现结点q，或者左子树出现结点q，
    //右子树出现结点p，那么该结点就是结点p和q的最近公共祖先
    //值得注意的是，本题函数有返回值，是因为回溯的过程需要递归函数的返回值做判断，但本题我们依然要遍历树的所有结点
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == q || root == p || root == null){
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if(left != null && right != null){ // 左右子树分别找到了，说明此时的 root 就是结果
            return root;
        }
        if(left == null && right != null){
            return right;
        }else if(left != null && right == null){
            return left;
        }else{//left == null && right == null
            return null;
        }
    }
}
//自己写的
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null){
            return null;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        //注意如果left不为空要及时返回，否则如例二，如果此处没有及时返回，
        //程序会继续向下执行，并返回3这个结点，而不是5这个结点
        if(left != null){
            return left;
        }
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if(right != null){
            return right;
        }
        if(judge(root, p) && judge(root, q)){
            return root;
        }
        return null;
    }

    public boolean judge(TreeNode root, TreeNode target){//判断以root为根节点的树是否包含target结点
        if(root == null){
            return false;
        }
        if(root.val == target.val){
            return true;
        }
        boolean left = judge(root.left, target);
        boolean right = judge(root.right, target);
        return left || right;
    }
}
```