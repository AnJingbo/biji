

![图示](https://img-blog.csdnimg.cn/193c3e65aec04d3c9bdb43e6c9db2fd0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 普通二叉树
```java
class Solution {
    public int countNodes(TreeNode root) {
        if(root == null){
            return 0;
        }
        return countNodes(root.left) + countNodes(root.right) + 1;
    }
}
```
* 时间复杂度为 O(N)
### 满二叉树
```java
class Solution {
    public int countNodes(TreeNode root) {
        int h = 0;
        // 计算树的高度
        while(root != null){
            root = root.left;
            h++;
        }
        // 结点总数就是 2^h - 1
        return (int)Math.pow(2, h) - 1;
    }
}
```
* 时间复杂度为 O(logN)
### 完全二叉树
```java
class Solution {
    public int countNodes(TreeNode root) {
        TreeNode l = root, r = root;
        // 记录左、右子树的高度
        int hl = 0, hr = 0;
        while(l != null){
            l = l.left;
            hl++;
        }
        while(r != null){
            r = r.right;
            hr++;
        }
        // 如果左右子树的高度相同，则是一棵满二叉树
        if(hl == hr){
            return (int)Math.pow(2, hl) - 1;
        }
        // 如果左右子树的高度不相同，则按照普通二叉树的逻辑计算
        return 1 + countNodes(root.left) + countNodes(root.right);
    }
}
```
* 时间复杂度为 O(logN * logN)
* 最后一行的两个递归中只有一个会真的递归下去，另一个一定会触发 hl == hr 而立即返回，不会递归下去。因为**一棵完全二叉树的两棵子树，至少有一棵是满二叉树。**