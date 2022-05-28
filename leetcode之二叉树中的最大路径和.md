

![图示](https://img-blog.csdnimg.cn/5459937a8b40465a9a582599acce8f7c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 当路径到达某个节点时，该路径既可以前往它的左子树也可以前往它的右子树，但如果路径同时经过它的左右子树，那么就不能经过它的父节点。
```java
class Solution {
    int maxSum = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        dfs(root);
        return maxSum;
    }
    // 返回以 root 为根结点的最大路径和
    public int dfs(TreeNode root){
        if(root == null) return 0;
        int left = dfs(root.left);
        int right = dfs(root.right);

        // 路径同时经过当前节点及其左右子树的情况。
        // 此时不能直接 return left + right + root.val; 因为 左根右 已经是一条完整的路径了，不能再加上父结点了，但是其结果有可能是全局最大路径和
        if(left >= 0 && right >= 0){
            maxSum = Math.max(maxSum, left + right + root.val);
        }else if(left < 0 && right < 0){ // 路径只包含当前节点
            maxSum = Math.max(maxSum, root.val);
            return root.val;
        }
        // 路径包含当前节点及其左右子树的任一子树
        maxSum = Math.max(maxSum, root.val + Math.max(left, right));
        return root.val + Math.max(left, right);
    }
}
```
* 简化后的代码：
```java
class Solution {
    int maxSum = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        dfs(root);
        return maxSum;
    }
    public int dfs(TreeNode root){
        if(root == null) return 0;
        int left = Math.max(dfs(root.left), 0);
        int right = Math.max(dfs(root.right), 0);

        maxSum = Math.max(maxSum, left + right + root.val);

        return root.val + Math.max(left, right);
    }
}
```