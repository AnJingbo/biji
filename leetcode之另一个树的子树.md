

```java
572. 另一个树的子树
给定两个非空二叉树 s 和 t，检验 s 中是否包含和 t 具有相同结构和节点值的子树。s 的一个子树包括 s 的一个节点和这个节点的所有子孙。s 也可以看做它自身的一棵子树。
示例 1:
给定的树 s:
           3
          / \
         4   5
        / \
       1   2
给定的树 t：
         4 
        / \
       1   2
返回 true。
示例 2:
给定的树 s：
           3
          / \
         4   5
        / \
       1   2
          /
         0
给定的树 t：
         4
        / \
       1   2
返回 false。
class Solution {
    public boolean isSubtree(TreeNode s, TreeNode t) {
        if(s == null){
            return t == null ? true : false;
        }
        boolean flag = isSame(s, t);
        boolean flag1 = isSubtree(s.left, t);
        boolean flag2 = isSubtree(s.right, t);
        return flag || flag1 || flag2;
    }
    //判断两颗树是否相等
    public boolean isSame(TreeNode s, TreeNode t){
       //首先排除空结点的情况
        if(s == null && t == null){
            return true;
        }else if(s != null && t == null){
            return false;
        }else if(s == null && t != null){
            return false;
        }else if(s.val != t.val){//排除结点均不为空但值不相同的情况
            return false;
        }
        boolean left = isSame(s.left, t.left);
        boolean right = isSame(s.right, t.right);
        return left && right;
    }
}
```