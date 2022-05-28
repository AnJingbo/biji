

```java
/*
108. 将有序数组转换为二叉搜索树
将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

示例:

给定有序数组: [-10,-3,0,5,9],

一个可能的答案是：[0,-3,9,-10,null,5]，它可以表示下面这个高度平衡二叉搜索树：

      0
     / \
   -3   9
   /   /
 -10  5
 */
 class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        if(nums.length == 0){
            return null;
        }
        //因为定义的区间是左闭右闭，所以是传入0和nums.length-1
        return traversal(nums, 0, nums.length - 1);
    }
    //确定递归函数的参数和返回值
    //删除二叉树结点，增加二叉树结点等都是用递归的返回值来完成，比较方便
    //本题要构造二叉树，依然要递归函数的返回值来构造中结点的左右孩子
    //注意要循环不变量：左闭右闭[left, right]
    public TreeNode traversal(int[] nums, int left, int right){
        //确定终止条件
        if(left > right){
            return null;
        }
        //确定单层递归的逻辑
        int mid = left + (right - left) / 2;//尽量不要写int mid = (left + right) / 2，这么写其实可能会造成数值越界，例如left和right都是int的最大值，这么操作就越界了 
        TreeNode root = new TreeNode(nums[mid]);
        root.left = traversal(nums, left, mid - 1);
        root.right = traversal(nums, mid + 1, right);
        return root;
    }
}
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        if(nums.length == 0){
            return null;
        }
        int mid = (0 + nums.length) / 2;
        TreeNode root = new TreeNode(nums[mid]);
        //左闭右开
        int[] left = Arrays.copyOfRange(nums, 0, mid);
        int[] right = Arrays.copyOfRange(nums, mid + 1, nums.length);
        root.left = sortedArrayToBST(left);
        root.right = sortedArrayToBST(right);
        return root;
    }
}
```