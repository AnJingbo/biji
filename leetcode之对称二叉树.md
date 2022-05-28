

```java
/*
给定一个二叉树，检查它是否是镜像对称的。
例如，二叉树 [1,2,2,3,4,4,3] 是对称的。
       1
      / \
     2   2
    / \ / \
   3  4 4  3
但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:
       1
      / \
     2   2
      \   \
      3    3
*/
class Solution {
    public boolean isSymmetric(TreeNode root) {
        //本题需要比较的其实是根节点的两个左右子树
        //本题遍历只能是后序，正是因为要遍历两棵树而且要比较内侧和外侧结点，所以准确来说是一个树
        //的遍历顺序是左右中，一个树的遍历顺序是右左中
        if(root == null){
            return true;
        }
        return compare(root.left, root.right);
    }
    public boolean compare(TreeNode left, TreeNode right){
        //终止条件
        //左为空，右不为空，不对称
        if(left == null && right != null){
            return false;
        }else if(left != null && right == null){//左不为空，右为空，不对称
            return false;
        }else if(left == null && right == null){//左右都为空，对称
            return true;
        }else if(left.val != right.val){//左右都不为空，值不同，不对称
            return false;
        }
        //确定单层递归的逻辑：即处理左右节点都不为空，而且数值相同的情况
        //比较二叉树外侧是否对称，传入的是c左节点的左孩子，右节点的右孩子
        //比较二叉树内侧是否对称，传入左节点的右孩子，右节点的左孩子
        //如果左右都对称就返回true，否则返回false
        boolean outside = compare(left.left, right.right);
        boolean inside = compare(left.right, right.left);
        boolean isSame = outside && inside;
        return isSame;
    }
}
```