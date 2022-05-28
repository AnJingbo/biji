

```java
/*
112. 路径总和
给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。

说明: 叶子节点是指没有子节点的节点。

示例: 
给定如下二叉树，以及目标和 sum = 22，
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
返回 true, 因为存在目标和为 22 的根节点到叶子节点的路径 5->4->11->2。
*/
//自己写的
class Solution {
    int count = 0;//定义全局变量记录结点值的和
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root == null){
            return false;
        }
        //遇到叶子结点并且此时和为sum，则返回true
        if(root.left == null && root.right == null){
            count += root.val;
            return count == sum ? true : false;
        }
        count += root.val;//把当前结点的值算入和中
        boolean flag1 = false;;
        boolean flag2 = false;;
        if(root.left != null){
            flag1 = hasPathSum(root.left, sum);
            count -= root.left.val;//回溯
        }
        if(root.right != null){
            flag2 = hasPathSum(root.right, sum);
            count -= root.right.val;//回溯
        }
        return flag1 || flag2;
    }
}
class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root == null){
            return false;
        }
        return traversal(root, sum - root.val);
    }
    //确定递归参数和返回值类型
    //参数：需要二叉树的根节点，还需要一个计数器，用来计算二叉树的一条边之和是否正好是目标和
    //返回值类型：如果需要搜索整颗二叉树，那么递归函数就不需要返回值。
    //如果要搜索其中一条符合条件的路径，那么就需要返回值，因为遇到符合条件的路径就要及时返回
    public boolean traversal(TreeNode root, int count){
        //确定终止条件
        //本题不要去累加然后判断是否等于目标和。可以用递减，让计数器count初始为目标和，然后每次减去遍历路径结点上的数值，如果最后count == 0，同时遇到了叶子结点的话，说明找到了目标和
        if(root.left == null && root.right == null){
            return count == 0 ? true : false;
        }
        //确定单层逻辑
        //因为终止条件是判断叶子结点，所以递归过程中就不要让空结点进入递归了
        if(root.left != null){
            count -= root.left.val;//处理结点
            if(traversal(root.left, count)){
                return true;
            }
            count += root.left.val;//回溯，撤销处理结果
        }
        if(root.right != null){
            count -= root.right.val;//处理结点
            if(traversal(root.right, count)){
                return true;
            }
            count += root.right.val;//回溯，撤销处理结果
        }
        return false;
    }
}
//非递归
class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root == null){
            return false;
        }
        //使用两个栈，一个存储要遍历的结点，另一个要存储从根节点到当前结点的路径和
        Stack<TreeNode> stack = new Stack<>();
        Stack<Integer> numStack = new Stack<>();
        stack.push(root);
        numStack.push(root.val);
        while(!stack.isEmpty()){
            TreeNode temp = stack.pop();
            int count = numStack.pop();
            if(temp.left == null && temp.right == null){
                if(count == sum){
                    return true;
                }
                continue;
            }
            /*精简之后：
            if(temp.left == null && temp.right == null && count == sum){
                    return true;
            }*/
            if(temp.right != null){
                stack.push(temp.right);
                numStack.push(count + temp.right.val);
            }
            if(temp.left != null){
                stack.push(temp.left);
                numStack.push(count + temp.left.val);
            }
        }
        return false;
    }
}
```