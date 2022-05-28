

![图示](https://img-blog.csdnimg.cn/1c1c8362dafe4896ad9366baa555cfef.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
非递归：

```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
    ListNode dummy = new ListNode();
    dummy.next = head;

    ListNode pre = dummy;
    ListNode end = dummy;

    while (end.next != null) {
        for (int i = 0; i < k; i++){
            // 判断必须在下面。因为如果在end等于null之后恰巧跳出循环了，就会出错
            end = end.next;
            if (end == null){
                return dummy.next;
            }
        }
        ListNode start = pre.next;
        ListNode next = end.next;
        pre.next = reverse(start, end.next);
        start.next = next;
        pre = start;
        end = pre;
    }
    return dummy.next;
}
    // 反转 [start, end) 区间的结点
    public ListNode reverse(ListNode start, ListNode end){
        ListNode cur = start;
        ListNode pre = null;
        while(cur != end){
            ListNode temp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = temp;
        }   
        return pre;
    }
}
```
非递归：
```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        if(head == null){
            return null;
        }
        ListNode start = head;
        ListNode end = head;
        for(int i = 0; i < k; i++){
            // 不足 k 个，不需要反转
            if(end == null){
                return head;
            }
            end = end.next;
        }
        // 反转前 k 个结点
        ListNode newHead = reverse(start, end);
        // 将后续的结点连接起来
        start.next = reverseKGroup(end, k);
        return newHead;
    }
    // 反转 [start, end) 区间的结点
    public ListNode reverse(ListNode start, ListNode end){
        ListNode cur = start;
        ListNode pre = null;
        while(cur != end){
            ListNode temp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = temp;
        }   
        return pre;
    }
}
```