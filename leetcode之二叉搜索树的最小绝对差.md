

![图示](https://img-blog.csdnimg.cn/bef1d49644e4471499c8dc0d0f9fdc84.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
* 前驱节点：对一棵二叉树进行中序遍历，遍历后的顺序，当前节点的前一个节点为该节点的前驱节点；
* 后继节点：对一棵二叉树进行中序遍历，遍历后的顺序，当前节点的后一个节点为该节点的后继节点；
```java
class Solution {
    TreeNode pre = null; // 用来记录前一个结点
    int res = Integer.MAX_VALUE;
    public int getMinimumDifference(TreeNode root) {
        if(root == null){
            return 0;
        }
        int left = getMinimumDifference(root.left);
        if(pre != null){
            res = Math.min(res, Math.abs(pre.val - root.val));
        }
        pre = root;
        int right = getMinimumDifference(root.right);
        return res;
    }
}
```