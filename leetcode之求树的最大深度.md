

```java
/*
104. 二叉树的最大深度
给定一个二叉树，找出其最大深度。
二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。
说明: 叶子节点是指没有子节点的节点。
示例：
给定二叉树 [3,9,20,null,null,15,7]，
    3
   / \
  9  20
    /  \
   15   7

*/
    public int maxDepth(TreeNode root) {
        //本题后序遍历(左右中)，因为要通过递归函数的返回值来计算树的高度
        //终止条件，如果为空结点的话，就返回0，f代表高度为0
        if(root == null){
            return 0;
        }
        //先求左子树深度，再求右子树深度，最后取左右深度最大再+1就是目前结点为根结点的树的深度
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        int depth = 1 + Math.max(left, right);
        return depth;
    }
  //以下代码是自己写的
    public int maxDepthMine(TreeNode root) {
        if(root == null){
            return 0;
        }
        return getDepth(root, 0);
    }
    public int getDepth(TreeNode root, int depth){
        if(root == null){
            return depth;
        }
        int left = getDepth(root.left, depth + 1);
        int right = getDepth(root.right, depth + 1);
        return left > right ? left : right;
    }
   //非递归（层序遍历）
   public int maxDepth(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        if(root != null){
            queue.add(root);
        }
        int depth = 0;
        while(!queue.isEmpty()){
            int size = queue.size();
            for(int i = 0; i < size; i++){
                TreeNode temp = queue.poll();
                if(temp.left != null){
                    queue.add(temp.left);
                }
                if(temp.right != null){
                    queue.add(temp.right);
                }
            }
            depth++;
        }
        return depth;
    }

```