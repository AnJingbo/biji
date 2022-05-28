

```java
/*
450. 删除二叉搜索树中的节点
给定一个二叉搜索树的根节点 root 和一个值 key，删除二叉搜索树中的 key 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

一般来说，删除节点可分为两个步骤：
              1、首先找到需要删除的节点；
              2、如果找到了，删除它。
说明： 要求算法时间复杂度为 O(h)，h 为树的高度。
示例:[2, 1, 7, null, null, 5, 9, 4, 6, 8, 10]
*/
class Solution {
    /*
      平衡二叉树删除结点遇到的情况：
          (1)没找到删除的结点，遍历到空结点则直接返回
          (2)左右孩子都为空(叶子结点)，直接删除结点，返回null作为根节点
          (3)删除结点的左孩子为空，右孩子不为空，删除结点，右孩子补位，返回右孩子为根节点
          (4)删除结点的右孩子为空，左孩子不为空，删除结点，左孩子补位，返回左孩子为根节点
          (5)左右孩子结点都不为空，则将删除结点的左子树头结点(左孩子)放到删除结点的右子树的最左面结点的左孩子上，返回删除结点右孩子为新的根结点
    */
    public TreeNode deleteNode(TreeNode root, int key) {
        if(root == null){//第一种：没找到要删除的结点，直接返回空结点
            return null;
        }
        if(root.val == key){
            //左右孩子都为空
            //左孩子为空，右孩子不为空
            if(root.left == null){
                return root.right;
            }else if(root.right == null){//右孩子为空，左孩子不为空
                return root.left;
            }else{//左右孩子结点都不为空
                TreeNode temp = root.right;
                while(temp.left != null){
                    temp = temp.left;
                }
                temp.left = root.left;//把要删除的结点的左子树放到要删除结点的右子树的最左面结点的左孩子上
                root = root.right;
                return root;//返回要删除节点的右孩子作为新的根结点
            }
        }
        if(root.val > key){
            root.left = deleteNode(root.left, key);
        }
        if(root.val < key){
            root.right = deleteNode(root.right, key);
        }
        return root;
    }
}
class Solution {
     public TreeNode deleteNode(TreeNode root, int key) {
        if(root == null){//没找到要删除的结点，直接返回空结点
            return null;
        }
        if(root.val == key){
            if(root.left == null && root.right == null){//左右孩子都为空
                return null;
            }else if(root.right != null && root.right != null){//左右孩子结点都不为空
                TreeNode temp = root.right;
                while(temp.left != null){
                    temp = temp.left;
                }
                temp.left = root.left;////把要删除的结点的左子树放到要删除结点的右子树的最左面结点的左孩子上
                return root.right;//返回要删除节点的右孩子作为新的根结点
            }else{//一个孩子结点为空
                if(root.left == null){//左孩子为空，右孩子不为空
                    return root.right;
                }else{//右孩子为空，左孩子不为空
                    return root.left;
                }
            }
        }
        if(root.val > key){
            root.left = deleteNode(root.left, key);
        }
        if(root.val < key){
            root.right = deleteNode(root.right, key);
        }
        return root;
    }
}
//迭代法
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if(root == null){
            return null;
        }
        TreeNode cur = root;
        TreeNode pre = null;
        while(cur != null){
            if(cur.val == key){
                break;
            }
            pre = cur;
            if(key < cur.val){
                cur = cur.left;
            }else{
                cur = cur.right;
            }
        }
        if(pre == null){//只有头结点
            return delNode(cur);
        }
        //pre要知道删除的是左孩子还是右孩子
        if(pre.left != null && pre.left.val == key){
            pre.left = delNode(cur);
        }
        if(pre.right != null && pre.right.val == key){
            pre.right = delNode(cur);
        }
        return root;
    }
    public TreeNode delNode(TreeNode node){
        if(node == null){
            return null;
        }
        if(node.right == null){
            return node.left;
        }
        TreeNode temp = node.right;
        while(temp.left != null){
            temp = temp.left;
        }
        temp.left = node.left;
        return node.right;
    }
}
```