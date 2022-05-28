

337. 打家劫舍 III
在上次打劫完一条街道之后和一圈房屋后，小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为“根”。 除了“根”之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果两个直接相连的房子在同一天晚上被打劫，房屋将自动报警。

计算在不触动警报的情况下，小偷一晚能够盗取的最高金额。
![图示](https://img-blog.csdnimg.cn/20210224145142516.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
暴力递归：

**本题一定是要后序遍历，因为通过递归函数的返回值来做下一步计算。**
```java
class Solution {
    public int rob(TreeNode root) {
        if(root == null){
            return 0;
        }
        if(root.left == null && root.right == null){
            return root.val;
        }
        int val1 = root.val;
        if(root.left != null){
            val1 += rob(root.left.left) + rob(root.left.right);
        }
        if(root.right != null){
            val1 += rob(root.right.left) + rob(root.right.right);
        }
        int val2 = rob(root.left) + rob(root.right);
        return Math.max(val1, val2);
        
    }
}
```
动态规划：

**1、确定递归函数的参数和返回值**

这里我们要求一个结点 偷与不偷 的两个状态所得到的金钱，那么返回值就是一个长度为2的数组。

参数为当前节点，代码如下：

```java
public int[] robTree(TreeNode root) 
```
其实这里的返回数组就是dp数组。

所以dp数组（dp table）以及下标的含义：**下标为0记录不偷该节点所得到的的最大金钱，下标为1记录偷该节点所得到的的最大金钱。**

所以本题dp数组就是一个长度为2的数组！

那么有同学可能疑惑，长度为2的数组怎么标记树中每个节点的状态呢？

别忘了在递归的过程中，系统栈会保存每一层递归的参数。

**2、确定终止条件**

在遍历的过程中，如果遇到空结点的话，很明显，无论偷还是不偷都是0，所以就返回

```java
if (root == null) return new int[]{0, 0};
```
这也相当于dp数组的初始化

**3、确定遍历顺序**

首先**明确的是使用后序遍历。因为通过递归函数的返回值来做下一步计算。**

通过递归左节点，得到左节点偷与不偷的金钱。

通过递归右节点，得到右节点偷与不偷的金钱。

代码如下：

```java
// 下标0：不偷，下标1：偷
int[] left = robTree(root->left); // 左
int[] right = robTree(root->right); // 右 
// 中
```

**4、确定单层递归的逻辑**

如果不偷当前结点，那么左右孩子就可以偷，至于到底偷不偷一定是选一个最大的，所以：val1 = max(left[0], left[1]) + max(right[0], right[1]);

如果是偷当前结点，那么左右孩子就不能偷，val2 = root->val + left[0] + right[0];  （如果对下标含义不理解就在回顾一下dp数组的含义）

最后当前结点的状态就是{val1, val2}; 即：{不偷当前节点得到的最大金钱，偷当前节点得到的最大金钱}

代码如下：

```java
int[] left = robTree(cur->left); // 左
int[] right = robTree(cur->right); // 右 

// 不偷root
int val1 = max(left[0], left[1]) + max(right[0], right[1]);
// 偷root
int val2 = cur->val + left[0] + right[0];
return new int[]{val1, val2};
```
**5、举例推导dp数组**
以示例1为例，dp数组状态如下：（注意用后序遍历的方式推导）
![图示1](https://img-blog.csdnimg.cn/20210224150431851.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
最后头结点就是 取下标0 和 下标1的最大值就是偷得的最大金钱。

```java
class Solution {
    public int rob(TreeNode root) {
        int[] res = robTree(root);
        return Math.max(res[0], res[1]);
    }
    public int[] robTree(TreeNode root){//长度为2的数组，0表示不偷，1表示偷
        if(root == null){
            return new int[]{0, 0};
        }
        int[] left = robTree(root.left);
        int[] right = robTree(root.right);
        //不偷root
        int val1 = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
        //偷root
        int val2 = root.val + left[0] + right[0];
        return new int[]{val1, val2};//{不偷，偷}
    }
}
```