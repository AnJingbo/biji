

![图示](https://img-blog.csdnimg.cn/6577c75502b1408c91524345fadffe78.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null){
            return 0;
        }
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        return Math.max(left, right) + 1;
    }
}
```
![图示](https://img-blog.csdnimg.cn/44e89a79447f453eb0c25ae4cb2cd241.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
注意：
![图示](https://img-blog.csdnimg.cn/a4c97d4ecef24e58a1221bf46ea6fb44.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
* 这就重新审题了，题目中说的是：**最小深度是从根节点到最近叶子节点的最短路径上的节点数量。，注意是叶子节点。**
* 什么是叶子节点，**左右孩子都为空的节点才是叶子节点！**
```java
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null){
            return 0;
        }
        int left = minDepth(root.left);
        int right = minDepth(root.right);
        // 左子树为空，右子树不为空，此时最小深度应该是右子树深度+1
        if(left == 0){
            return right + 1;
        }
        // 右子树为空，左子树不为空，此时最小深度应该是左子树深度+1
        if(right == 0){
            return left + 1;
        }
        // 左右子树都不为空
        return Math.min(left, right) + 1;
    }
}
```