

![图示](https://img-blog.csdnimg.cn/cdb586f8dd7346da903f2254b961ae92.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_19,color_FFFFFF,t_70,g_se,x_16)
* 前驱结点：对一棵二叉树进行中序遍历，遍历后的顺序，当前结点的前一个结点为该结点的前驱结点；
* 后继结点：对一棵二叉树进行中序遍历，遍历后的顺序，当前结点的后一个结点为该结点的后继结点；
```java
class Solution {
    Node pre = null, head = null;
    public Node treeToDoublyList(Node root) {
        if(root == null){
            return null;
        }
        traversal(root);
        // 中序遍历完成后，head 指向头结点， pre 指向尾结点
        head.left = pre;
        pre.right = head; 
        return head;
    }
    public void traversal(Node cur){
        if(cur == null){
            return;
        }
        traversal(cur.left);

        // 当 pre 为空时：代表正在访问链表头节结点，记为 head
	    // 当 pre 不为空时：修改双向结点引用，即 pre.right = cur; cur.left = pre;
        if(pre != null){
            pre.right = cur;
            // cur.left = pre;
        }else {
            head = cur;
        }
        cur.left = pre;
        pre = cur; // pre 是 cur 的前驱结点，即前一个结点。比如本例中 1 是 pre，2 是 cur

        traversal(cur.right);
    }
}
```