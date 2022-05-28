

![图示](https://img-blog.csdnimg.cn/3eaa5805294e4cf38afb62063fdfd72d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
* 第一种思路：可以将数组中的链表两两合并（**leetcode21 合并两个有序链表**），最终得到正确答案。
* 第二种思路：使用优先队列（二叉堆）进行合并。
```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists.length == 0){
            return null;
        }
        ListNode head = new ListNode();
        ListNode temp = head;
        // 优先队列，小顶堆
        PriorityQueue<ListNode> pq = new PriorityQueue<>(lists.length, (a, b) -> (a.val - b.val));// ListNode 是自定义的引用数据类型，因此要指定如何排序
        // 将数组中的链表加入到优先队列中。
        for(ListNode list : lists){
            if(list != null){
                pq.add(list);
            }
        }
        while(!pq.isEmpty()){
            // 获得最小的结点，接到结果链表中
            ListNode node = pq.poll();
            temp.next = node;
            temp = temp.next;
            if(node.next != null){
                pq.add(node.next);
            }
        }
        return head.next;
    }
}
```
* 第三种思路：分治思想
```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists.length == 0){
            return null;
        }
        return merge(0, lists.length - 1, lists);
    }
    public ListNode merge(int left, int right, ListNode[] lists){
        if(left == right){
            return lists[left];
        }
        int mid = left + (right - left) / 2;
        ListNode leftList = merge(left, mid, lists);
        ListNode rightList = merge(mid + 1, right, lists);
        return mergeTwo(leftList, rightList);
    }
    public ListNode mergeTwo(ListNode l1, ListNode l2){
        ListNode head = new ListNode();
        ListNode temp = head;
        while(l1 != null && l2 != null){
            if(l1.val <= l2.val){
                temp.next = l1;
                temp = temp.next;
                l1 = l1.next;
            }else{
                temp.next = l2;
                temp = temp.next;
                l2 = l2.next;
            }
        }
        if(l1 != null){
            temp.next = l1;
        }
        if(l2 != null){
            temp.next = l2;
        }
        return head.next;
    }
}
```