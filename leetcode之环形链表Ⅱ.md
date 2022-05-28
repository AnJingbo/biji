

```java
/**
 * 142. 环形链表 II
 * 给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。
 * 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意，pos 仅仅是用于标识环的情况，并不会作为参数传递到函数中。
 * 说明：不允许修改给定的链表。
 *
 * 示例 1：
 *
 * 输入：head = [3,2,0,-4], pos = 1
 * 输出：返回索引为 1 的链表节点
 * 解释：链表中有一个环，其尾部连接到第二个节点。
 * 
 * 示例2：
 * 输入：head = [1,2], pos = 0
 * 输出：返回索引为 0 的链表节点
 * 解释：链表中有一个环，其尾部连接到第一个节点。
 * 
 * 示例3：
 * 输入：head = [1], pos = -1
 * 输出：返回 null 
 * 解释：链表中没有环。
 */
class ListNode {
    int val;
    ListNode next;

    public ListNode(int val) {
        this.val = val;
    }
}

/*
    1、判断链表是否有环
      解决：可以使用快慢指针法，分别定义fast(一次移动两个结点)和slow(一次移动一个结点)指针，
     从头结点出发，如果fast和slow指针在中途相遇，说明这个链表又环
    2、如果有环，如何找到这个环的入口
      解决：由相关数学知识可得：从头结点出发一个指针，从相遇结点也出发一个指针，这两个指针每
     次只走一个结点，那么当这两个指针相遇的时候就是环形入口的结点
*/
public class Ti19 {
    public static void main(String[] args) {
        ListNode head = new ListNode(3);
        ListNode node1 = new ListNode(2);
        ListNode node2 = new ListNode(0);
        ListNode node3 = new ListNode(4);
        head.next = node1;
        node1.next = node2;
        node2.next = node3;
        node3.next = node1;
        ListNode res = detectCycle(head);
        System.out.println("该链表中循环链表的第一个结点的值是: " + res.val);
    }

    public static ListNode detectCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {
                ListNode index1 = fast;
                ListNode index2 = head;
                while (index1 != index2) {
                    index1 = index1.next;
                    index2 = index2.next;
                }
                return index2;
            }
        }
        return null;
    }
}
```