

### 147. 对链表进行插入排序
![图示](https://img-blog.csdnimg.cn/41179a390d5d4a878f503075b90bdcd2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public ListNode insertionSortList(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        ListNode last = head; // 指向链表已排序部分的最后一个结点
        ListNode cur = head.next; // 指向待插入的元素
        while(cur != null){
            if(last.val <= cur.val){
                last = last.next;
            }else{
                ListNode pre = dummyHead; // 指向 cur 要插入位置的前一个位置
                while(pre.next != null && pre.next.val < cur.val){
                    pre = pre.next;
                }
                last.next = cur.next;
                cur.next = pre.next;
                pre.next = cur;
            }
            cur = last.next;
        }
        return dummyHead.next;
    }
}
```
### 148. 排序链表
![图示](https://img-blog.csdnimg.cn/2449257258f74095b9af13393645c027.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public ListNode sortList(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode fast = head, slow = head;
        // 注意：不能是 while(fast != null && fast.next != null)，否则会无限递归。比如当链表为：4->2->null 时，就会出错。
        while(fast.next != null && fast.next.next != null){
            fast = fast.next.next;
            slow = slow.next;
        }
        ListNode node = slow.next;
        slow.next = null;
        ListNode left = sortList(head);
        ListNode right = sortList(node);
        return merge(left, right);
    }
    public ListNode merge(ListNode left, ListNode right){
        ListNode dummyHead = new ListNode(0);
        ListNode cur = dummyHead;
        while(left != null && right != null){
            if(left.val < right.val){
                cur.next = left;
                left = left.next;
            }else{
                cur.next = right;
                right = right.next;
            }
            cur = cur.next;
        }
        if(left != null){
            cur.next = left;
        }
        if(right != null){
            cur.next = right;
        }
        return dummyHead.next;
    }
}
```
* 时间复杂度 $O(nlogn)$，空间复杂度 $O(logn)$