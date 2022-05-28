

```java
/*
513. 找树左下角的值
给定一个二叉树，在树的最后一行找到最左边的值。
示例 1:
 输入:
        2
       / \
      1   3
 输出: 1
示例 2:
 输入:
        1
       / \
      2   3
     /   / \
    4   5   6
       /
      7
输出: 7
*/
class Solution {
    //如何判断最后一行：其实深度最大的叶子结点一定是最后一行
    int maxDepth = -1;//记录最大深度
    int maxLeftValue;//记录最大深度最左结点的值
    public int findBottomLeftValue(TreeNode root) {
        if(root == null){
            return 0;
        }
        traversal(root, 0);
        return maxLeftValue;
    }
    //如果需要遍历整棵树，递归函数就不能有返回值。
    //如果需要遍历某一条固定的路线，递归函数就一定要有返回值。
    public void traversal(TreeNode root, int depth){
        //确定终止条件
        //当遇到叶子结点的时候，就需要统计一下最大深度了
        if(root.left == null && root.right == null){
            if(depth > maxDepth){
                maxDepth = depth;
                maxLeftValue = root.val;
            }
            return;
        }
        //确定单层递归的逻辑
        //注意：回溯和递归是一一对应的，有一个递归，就要有一个回溯，不能拆开写
        /*if(root.left != null){
            depth++;
            traversal(root.left, depth);
            depth--;//回溯
        }
        if(root.right != null){
            depth++;
            traversal(root.right, depth);
            depth--;//回溯
        }*/
        //简化：
        if(root.left != null){
            traversal(root.left, depth + 1);//隐藏着回溯
        }
        if(root.right != null){
            traversal(root.right, depth + 1);//隐藏着回溯
        }
        return;
    }
}
//层序遍历
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        int res = 0;
        if(root != null){
            queue.add(root);
        }
        while(!queue.isEmpty()){
            int size = queue.size();
            for(int i = 0; i < size; i++){
                TreeNode node = queue.poll();
                if(i == 0){
                    res = node.val;
                }
                if(node.left != null){
                    queue.add(node.left);
                }
                if(node.right != null){
                    queue.add(node.right);
                }
            }
        }
        return res;
    }
}
```