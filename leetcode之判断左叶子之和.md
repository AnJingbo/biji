

```java
/*
404. 左叶子之和
计算给定二叉树的所有左叶子之和。
示例：
         3
        / \
       9  20
         /  \
         5   7
在这个二叉树中，有两个左叶子，分别是 9 和 15，所以返回 24
*/
class Solution {
   //确定递归函数的参数和返回值
    public int sumOfLeftLeaves(TreeNode root) {
       //确定终止条件
        if(root == null){
            return 0;
        }
        //确定单层递归的逻辑
        //遇到左叶子结点，记录数值，然后通过递归求取左子树的左叶子之和
        //以及右叶子之和，相加便是整个树的左叶子之和
        int leftValue = sumOfLeftLeaves(root.left);
        int rightValue = sumOfLeftLeaves(root.right);
        int midValue = 0;
        if(root.left != null && root.left.left == null && root.left.right == null){
            midValue = root.left.val;
        }
        int sum = midValue + leftValue + rightValue;
        return sum;
    }
    //层序遍历
    public int sumOfLeftLeaves(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        int sum = 0;
        if(root != null){
            queue.add(root);
        }
        while(!queue.isEmpty()){
            int size = queue.size();
            for(int i = 0; i < size; i++){
                TreeNode temp = queue.poll();
                if(temp.left != null){
                    if(temp.left.left == null && temp.left.right == null){
                        sum += temp.left.val;
                    }
                    queue.add(temp.left);
                }
                if(temp.right != null){
                    queue.add(temp.right);
                }
            }
        }
        return sum;
    }
```