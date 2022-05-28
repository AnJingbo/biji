

### 124. 二叉树中的最大路径和
![ts](https://img-blog.csdnimg.cn/69ce67ba7c37498f87dc8bdcc1cadb1f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
```java
/*
    a
   / \
  b   c
(1) b + a + c。
(2) b + a + a 的父结点
(3) a + c + a 的父结点
情况 1，表示如果不联络父结点的情况，或本身是根结点的情况。这种情况是没法递归的，但是结果有可能是全局最大路径和。
情况 2 和 3，递归时计算 a+b 和 a+c，选择一个更优的方案返回，也就是上面说的递归后的最优解啦。
*/
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
        // 此时不能直接 return left + right + root.val; 因为 左根右 已经是一条完整的路径了，不能再加上父结点了。但是其结果有可能是全局最大路径和
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
### 687. 最长同值路径
![ts](https://img-blog.csdnimg.cn/8d7c423873ef4dc2ab96bd2cc9fd86ba.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
```java
/*
    a
   / \
  b   c
(1) b + a + c。
(2) b + a + a 的父结点
(3) a + c + a 的父结点
情况 1，表示如果不联络父结点的情况，或本身是根结点的情况。这种情况是没法递归的，但是结果有可能是最长同值路径
情况 2 和 3，递归时计算 a - b 和 a - c，选择一个更优的方案返回，也就是上面说的递归后的最优解啦。
*/
class Solution {
    int res = 0;
    public int longestUnivaluePath(TreeNode root) {
        if(root == null) return 0;
        dfs(root);
        return res;
    }
    public int dfs(TreeNode root){
        if(root == null) return 0;
        int l = dfs(root.left);
        int r = dfs(root.right);
        if(root.left != null && root.val == root.left.val){
            l++;
        }else{
            l = 0;
        }
        if(root.right != null && root.val == root.right.val){
            r++;
        }else{
            r = 0;
        }
        res = Math.max(res, l + r);
        return Math.max(l, r);
    }
}
```