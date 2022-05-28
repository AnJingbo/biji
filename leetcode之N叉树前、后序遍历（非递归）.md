

![leetcode之N叉树前序遍历](https://img-blog.csdnimg.cn/20201101170306162.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70#pic_center)

```java
public List<Integer> preorder(Node root) {
        List<Integer> res = new ArrayList<>();
        Stack<Node> stack = new Stack<>();
        stack.add(root);
        while(!stack.isEmpty()){
            Node temp = stack.pop();
            if(temp != null){
                res.add(temp.val);
            }else continue;
            //从右到左压栈，在出栈时才能从左到右
            for(int i = temp.children.size() - 1; i >= 0; i--){
                if(temp.children.get(i) != null){
                    stack.push(temp.children.get(i));
                }
            }
        }
        return res;
    }
    //参考二叉树前序遍历非递归写法
     public static void preTraverseOther(TrNode root){
        Stack<TrNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TrNode temp = stack.pop();
            if(temp != null){
                System.out.println(temp);
            }else continue;
            //先加右孩子，再加左孩子，是因为这样出栈的时候才是中左右的顺序
            stack.push(temp.right);
            stack.push(temp.left);
        }
    }
```

![leetcode之N叉树后序遍历](https://img-blog.csdnimg.cn/20201101171238115.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70#pic_center)

```java
public List<Integer> postorder(Node root) {
        List<Integer> res = new ArrayList<>();
        Stack<Node> stack = new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()){
            Node temp = stack.pop();
            if(temp != null){
                res.add(temp.val);
            }else continue;
            for(Node node : temp.children){
                stack.push(node);
            }
        }
        Collections.reverse(res);
        return res;
    }
}
//参考二叉树后序遍历非递归写法
public static List<Integer> postTraverseOther(TrNode root){
        List<Integer> res = new ArrayList<>();
        Stack<TrNode> stack = new Stack<>();
        stack.push(root);
        //先序遍历(中左右)---> 中右左--->翻转res变成 左右中
        while (!stack.isEmpty()) {
            TrNode temp = stack.pop();
            if(temp != null){
                res.add(temp.val);
            }else continue;
            stack.push(temp.left);
            stack.push(temp.right);
        }
        Collections.reverse(res);
        for(int i : res){
            System.out.println("val = " + i);
        }
        return res;
    }
```