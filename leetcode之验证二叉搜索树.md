

![图示](https://img-blog.csdnimg.cn/0ea6848dd8d940d5ac6c16794f14dc88.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_17,color_FFFFFF,t_70,g_se,x_16)
### 中序遍历
* 前驱节点：对一棵二叉树进行中序遍历，遍历后的顺序，当前节点的前一个节点为该节点的前驱节点；
* 后继节点：对一棵二叉树进行中序遍历，遍历后的顺序，当前节点的后一个节点为该节点的后继节点；
```java
class Solution {
    TreeNode pre = null; // 用来记录前一个结点
    public boolean isValidBST(TreeNode root) {
        if(root == null){
            return true;
        }
        boolean left = isValidBST(root.left);
        if(pre != null && pre.val >= root.val){
            return false;
        }
        pre = root;
        boolean right = isValidBST(root.right);
        return left && right;
    }
}
```
### 后序遍历
* 注意：**我们要比较的是左子树所有节点小于中间节点，右子树所有节点大于中间节点**，而**不能单纯地比较左节点小于中间节点，右节点大于中间节点就完事了。**
* 如下图中就会出现错误：
![图示](https://img-blog.csdnimg.cn/eb96c39e8d6c40c2b9907ee75d6b5ddf.png)
```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        if(root == null){
            return true;
        }
        //思路：后序遍历，从下往上判断，如果下面就出现了不是二叉搜索树的，直接返回false，如果下面的是二叉搜索树，那么就比较依次向上走。
        boolean left = isValidBST(root.left);
        boolean right = isValidBST(root.right);
        //判断根节点是否比其左子树的所有结点都大
        TreeNode temp = root.left;
        while(temp != null){
            if(temp.val >= root.val){
                return false;
            }else{
                temp = temp.right;
            }
        }
        //判断根节点是否比其右子树的所有结点都小
        temp = root.right;
        while(temp != null){
            if(temp.val <= root.val){
                return false;
            }else{
                temp = temp.left;
            }
        }
        return left && right;
    }
}
```