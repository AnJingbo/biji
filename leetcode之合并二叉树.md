

```java
/*
617. 合并二叉树
你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为 NULL 的节点将直接作为新二叉树的节点。
示例 1:
输入: 
	Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  
输出: 
合并后的树:
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7
*/

class Solution {
    //确定递归函数的参数和返回值
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        //确定终止条件
        if(t1 == null){//如果t1为空，那么合并之后就应该是t2
            return t2;
        }
        if(t2 == null){//如果t2为空，那么合并之后就应该是t1
            return t1;
        }
        //确定单层递归的逻辑
        t1.val += t2.val;
        t1.left = mergeTrees(t1.left, t2.left);
        t1.right = mergeTrees(t1.right, t2.right);
        return t1;
    }
}
//自己写的
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        TreeNode root = null;
        if(t1 == null && t2 == null){
            return root;
        }else if(t1 == null && t2 != null){//如果某颗子树为null，那么直接将该子树的根赋给root，然后返回
            root = t2;
            return root;
        }else if(t1 != null && t2 == null){
            root = t1;
            return root;
        }
        root = new TreeNode(t1.val + t2.val);
        root.left = mergeTrees(t1.left, t2.left);
        root.right = mergeTrees(t1.right, t2.right);
        return root;
    }
}
//非递归
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if(t1 == null){
            return t2;
        }
        if(t2 == null){
            return t1;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(t1);
        queue.add(t2);
        while(!queue.isEmpty()){
            TreeNode temp1 = queue.poll();
            TreeNode temp2 = queue.poll();
            //此时temp1和temp2一定不为空
            temp1.val += temp2.val;
            if(temp1.left != null && temp2.left != null){
                queue.add(temp1.left);
                queue.add(temp2.left);
            }
            if(temp1.right != null && temp2.right != null){
                queue.add(temp1.right);
                queue.add(temp2.right);
            }
            if(temp1.left == null && temp2.left != null){
                temp1.left = temp2.left;
            }
            if(temp1.right == null && temp2.right != null){
                temp1.right = temp2.right;
            }
        }
        return t1;
    }
}
```