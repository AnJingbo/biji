

```java
/*
538. 把二叉搜索树转换为累加树
给出二叉 搜索 树的根节点，该树的节点值各不相同，请你将其转换为累加树（Greater Sum Tree），使每个节点 node 的新值等于原树中大于或等于 node.val 的值之和。
提醒一下，二叉搜索树满足下列约束条件：
节点的左子树仅包含键 小于 节点键的节点。
节点的右子树仅包含键 大于 节点键的节点。
左右子树也必须是二叉搜索树。
示例：
输入：[4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
输出：[30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
*/
class Solution {
    //二叉搜索树换个角度看，其实就是一个有序数组，例如[2, 5, 13]，求从后到前的累加数组，也就是[20, 18, 13]。
    //换成数组就简单多了，是因为大家都知道数组怎么遍历，从后向前，依次累加即可
    //因此知道怎么遍历这个二叉树，问题即可迎刃而解。
    //从树中可以看出累加的顺序是右中左，所以我们需要反中序遍历这个二叉树，然后顺序累加就可以了
    TreeNode pre = null;//记录当前遍历结点的前一个结点
    public TreeNode convertBST(TreeNode root) {
        if(root == null){
            return null;
        }
        convertBST(root.right);
        if(pre != null){
            root.val += pre.val;
        }
        pre = root;
        convertBST(root.left);
        return root;
    }
}
//迭代法
class Solution {
    public TreeNode convertBST(TreeNode root) {
        TreeNode pre = null;
        TreeNode temp = root;
        Stack<TreeNode> stack = new Stack<>();
        while(!stack.isEmpty() || temp != null){
            if(temp != null){
                stack.push(temp);
                temp = temp.right;
            }else{
                temp = stack.pop();
                if(pre != null){
                    temp.val += pre.val; 
                }
                pre = temp;
                temp = temp.left;
            }
        } 
        return root;
    }
}
```