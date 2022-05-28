

```java
/*
106. 从中序与后序遍历序列构造二叉树
根据一棵树的中序遍历与后序遍历构造二叉树。
注意:你可以假设树中没有重复的元素。
例：
中序遍历 inorder = [9,3,15,20,7]
后序遍历 postorder = [9,15,7,20,3]
返回如下的二叉树：
                 3
                / \
               9  20
                 /  \
                15   7
*/
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        //第一步：如果数组大小为0，说明是空树
        if(inorder.length == 0 || postorder.length == 0){
            return null;
        }
        //第二步：如果不为空，那么取后序数组最后一个元素作为节点元素
        int rootVal = postorder[postorder.length - 1];
        TreeNode root = new TreeNode(rootVal);
        //叶子结点
        if(inorder.length == 1){
            return root;
        }
        //第三步：找到后序数组的最后一个元素在中序数组的位置，作为切割点
        int delimiterIndex;
        for(delimiterIndex = 0; delimiterIndex < inorder.length; delimiterIndex++){
            if(inorder[delimiterIndex] == rootVal){
                break;
            }
        }
        //切割过程中会产生四个区间，所以要循环不变量，否则会乱套。本例中每次使用的都是左闭右开
        //第四步：切割中序数组，切成中序左数组和中序右数组(顺序不能搞反，一定是先切中序数组，因为切割点在后序数组的最后一个元素，就是用这个元素来切割中序数组)
        //左闭右开区间:[0, delimiterIndex)
        int[] leftInorder = Arrays.copyOfRange(inorder, 0, 0 + delimiterIndex);
        //[delimiterIndex + 1, inorder.length)
        int[] rightInorder = Arrays.copyOfRange(inorder, 0 + delimiterIndex + 1, inorder.length);
        //postorder 舍弃末尾元素（因为末尾元素不能要了，这是切割点也是当前二叉树中间结点的元素，已经用了）
        int[] temp = Arrays.copyOfRange(postorder, 0, postorder.length - 1);
        postorder = temp;
        //第五步：切割后序数组，切成后序左数组和后序右数组
        //左闭右开区间:[0, leftInorder.length),注意这里使用了左中序数组大小作为切割点（因为中序数组的大小一定和后序数组的大小相同）
        int[] leftPostorder = Arrays.copyOfRange(postorder, 0, 0 + leftInorder.length);
        //左闭右开区间:[leftInorder.length, postorder.length)
        int[] rightPostorder = Arrays.copyOfRange(postorder, 0 + leftInorder.length, postorder.length);

        //第六步：递归处理左区间和右区间
        root.left = buildTree(leftInorder, leftPostorder);
        root.right = buildTree(rightInorder, rightPostorder);
        return root;
    }
}
```