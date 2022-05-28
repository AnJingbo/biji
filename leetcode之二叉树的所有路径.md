

```java
/*
257. 二叉树的所有路径
给定一个二叉树，返回所有从根节点到叶子节点的路径。
说明: 叶子节点是指没有子节点的节点。
示例:
输入:
      1
    /   \
   2     3
    \
     5
输出: ["1->2->5", "1->3"]
解释: 所有根节点到叶子节点的路径为: 1->2->5, 1->3
*/
class Solution {
   //本题要涉及回溯，因为我们要把路径记录下来，再通过回溯来回退一个路径并进入到另一个路径
    public List<String> binaryTreePaths(TreeNode root) {
        if(root == null){
            return new ArrayList<>();
        }
        List<String> res = new ArrayList<>();
        traversal(root, "", res);
        return res;
    }
    /**
    root: 根节点
    path: 记录每一条路径
    res: 存放结果集
    */
    public void traversal(TreeNode root, String path, List<String> res){
        //终止条件
        //如果root为空，直接返回
        if(root == null){
            return;
        }
        //什么时候找到叶子结点？叶子结点：当前结点不为空，其左右孩子都为空时，就找到了叶子结点
        if(root.left == null && root.right == null){
            path += root.val;
            res.add(path);
            return;
        }
        path += root.val + "->";
        //以下过程体现了回溯的过程。
        //回溯和递归要永远在一起，回溯和递归是一一对应的，有一个递归，就要有一个回溯
        traversal(root.left, path, res);//注意传入的是当前root下的path，暗含了回溯！！！
        traversal(root.right, path, res);
    }
    public void traversal(TreeNode root, String path, List<String> res){
        //终止条件
        //如果root为空，直接返回
        if(root == null){
            return;
        }
        //什么时候找到叶子结点？叶子结点：当前结点不为空，其左右孩子都为空时，就找到了叶子结点
        if(root.left == null && root.right == null){
            res.add(path + root.val);
            return;
        }
        //回溯隐藏在path + root.val + "->"中，每次函数调用完，path依然没有加上"->"，这就是回溯了
        //回溯和递归要永远在一起，回溯和递归是一一对应的，有一个递归，就要有一个回溯
        traversal(root.left, path + root.val + "->", res);
        traversal(root.right, path + root.val + "->", res);
    }
    //非递归
    public List<String> binaryTreePaths(TreeNode root) {
        if(root == null){
            return new ArrayList<>();
        }
        List<String> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        Stack<String> pathStack = new Stack<>();
        stack.push(root);
        pathStack.push(root.val + "");
        while(!stack.isEmpty()){
            TreeNode temp = stack.pop();
            String path = pathStack.pop();
            if(temp.left == null && temp.right == null){
                res.add(path);
            }
            if(temp.right != null){
                stack.push(temp.right);
                pathStack.push(path + "->" + temp.right.val);//注意这里的path的值没有变
            }
            if(temp.left != null){
                stack.push(temp.left);
                pathStack.push(path + "->" + temp.left.val);//注意这里path的值没有变
            }
        }
        return res;
    }

}
```