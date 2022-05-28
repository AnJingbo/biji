

### 用栈实现队列
![图示](https://img-blog.csdnimg.cn/e3c5a5fef9ed4c9ab89be33a986004e8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
```java
class MyQueue {
    // s1 栈用来支持插入操作
    Stack<Integer> s1 = new Stack<>();
    // s2 栈用来支持删除操作
    Stack<Integer> s2 = new Stack<>();

    public MyQueue() {}
    
    public void push(int x) {
        s1.push(x);
    }
    
    public int pop() {
        if(s1.isEmpty() && s2.isEmpty()){
            return -1;
        }
        if(s2.isEmpty()){
            // 把 s1 中的元素压入到 s2 中
            while(!s1.isEmpty()){
                s2.push(s1.pop());
            }
        }
        return s2.pop();
    }
    
    public int peek() {
        if(s1.isEmpty() && s2.isEmpty()){
            return -1;
        }
        if(!s2.isEmpty()){
            return s2.peek();
        }
        if(!s1.isEmpty()){
            int res = pop();
            s2.add(res);
            return res;
        }
        return s2.peek();
    }
    
    public boolean empty() {
        return s1.isEmpty() && s2.isEmpty();
    }
}
```
### 用队列实现栈
![图示](https://img-blog.csdnimg.cn/9d34b46734114d95bdd2142ecab98728.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
```java
class MyStack {
    Queue<Integer> queue = new LinkedList<>();
    
    public MyStack() {}
    
    public void push(int x) {
        queue.add(x);
        int size = queue.size();
        while(size > 1){
            queue.add(queue.poll());
            size--;
        }
    }
    
    public int pop() {
        return queue.poll();
    }
    
    public int top() {
        return queue.peek();
    }
    
    public boolean empty() {
        return queue.isEmpty();
    }
}
```
### 包含 min 函数的栈
![图示](https://img-blog.csdnimg.cn/167a937450cf4921a8a8293bfd1849f1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
本题中，如果只用一个 min 来记录当前最小元素，则没有考虑出栈时，怎么对 min 进行更新。例如：依次压入 1, 2, 3，此时 min 的值是 1，但如果将 1 弹出栈后无法再更新 min 的值为 2。**为了解决出栈时对最小值的更新问题， 可以设置一个辅助栈。**
```java
class MinStack {
    Stack<Integer> s1 = new Stack<>(); // 数据栈
    Stack<Integer> s2 = new Stack<>(); // 辅助栈，栈顶表示当前数据栈的最小值

    public MinStack() {}
    
    // 每次有元素入栈，就将当前最小值入辅助栈
    public void push(int x) {
        s1.push(x);
        // 如果辅助栈为空，或者最小值大于 x，则将最小值设置为 x
        if(s2.isEmpty() || s2.peek() > x){
            s2.push(x);
        }else{
            // 如果最小值小于 x，则继续将该最小值压入辅助栈
            s2.push(s2.peek());
        }
    }
    // 出栈时，辅助栈也要将栈顶元素出栈
    public void pop() {
        s1.pop();
        s2.pop();
    }
    
    public int top() {
        return s1.peek();
    }
    
    public int min() {
        return s2.peek();
    }
}
```
### 反转链表
![图示](https://img-blog.csdnimg.cn/89005b6e8f324d8ba491c2e440e6044f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while(cur != null){
            ListNode temp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = temp;
        }
        return pre;
    }
}
```
递归：
```java
class Solution {
    public ListNode reverseList(ListNode head) { // 反转以 head 为头结点的链表
        if(head == null || head.next == null){
            return head;
        }
        ListNode res = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return res;
    }
}
```
### 复杂链表的复制
![图示](https://img-blog.csdnimg.cn/2ea056d3cbe2429fb9aacc7c3a784755.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public Node copyRandomList(Node head) {
        if(head == null){
            return head;
        }
        Map<Node, Node> map = new HashMap<>();
        Node cur = head;
        // 建立 旧结点->新节点 的映射
        while(cur != null){
            map.put(cur, new Node(cur.val));
            cur = cur.next;
        }
        cur = head;
        while(cur != null){
            map.get(cur).next = map.get(cur.next);
            map.get(cur).random = map.get(cur.random);
            cur = cur.next;
        }
        return map.get(head);
    }
}
```
### 二维数组中的查找
![图示](https://img-blog.csdnimg.cn/78f8cda5987943cb87066af231168a82.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        int i = matrix.length - 1, j = 0;
        while(i >= 0 && j < matrix[0].length){
            if(matrix[i][j] == target){
                return true;
            }else if(matrix[i][j] > target){
                i--;
            }else{
                j++;
            }
        }
        return false;
    }
}
```
### 连续子数组的最大和
![图示](https://img-blog.csdnimg.cn/0fffc1a8ffac4edcafce3b4443cb5ea6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public int maxSubArray(int[] nums) {
        // 以下标为 i 的数结尾的子数组的最大和。以下标为 i 的数结尾说明一定包含 nums[i] 这个数
        int[] dp = new int[nums.length];

        dp[0] = nums[0];
        int res = nums[0];
        for(int i = 1; i < nums.length; i++){
            // nums[i]单独成为一段 or 加入之前的那一段，这取决于两者谁大谁小
            dp[i] = Math.max(dp[i - 1] + nums[i], nums[i]);
            res = Math.max(res, dp[i]);
        }
        return res;
    }
}
```
### 两个链表的第一个公共节点
![图示](https://img-blog.csdnimg.cn/e7a3d1d6fba2489ebdce8de55adf263c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
（1）方法 1：哈希集合
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        HashSet<ListNode> set = new HashSet<>();
        while(headA != null){
            set.add(headA);
            headA = headA.next;
        }
        while(headB != null){
            if(set.contains(headB)){
                return headB;
            }
            headB = headB.next;
        }
        return null;
    }
}
```
（2）方法 2：双指针。
思路：使用两个指针 a，b 分别指向两个链表 headA，headB 的头结点，然后同时遍历，当 a 到达链表 headA 的末尾时，重新定位到链表 headB 的头结点，然后继续遍历；当 b 到达链表 headB 的末尾时，重新定位到链表 headA 的头结点，然后继续遍历。这样，当它们相遇时，所指向的结点就是第一个公共结点。
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode a = headA;
        ListNode b = headB;
        while(a != b){
            a = a != null ? a.next : headB;
            b = b != null ? b.next : headA;
        }
        return a;
    }
}
```
### 翻转单词顺序
![图示](https://img-blog.csdnimg.cn/d7bf3a4558c54349b7a2f4be821e279c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public String reverseWords(String s) {
        s = s.trim(); // 删除首尾空格
        int i = s.length() - 1, j = s.length() - 1;
        StringBuilder res = new StringBuilder("");
        while(i >= 0){
            while(i >= 0 && s.charAt(i) != ' '){ // 搜索空格
                i--;
            }
            res.append(s.substring(i + 1, j + 1) + " ");

            while(i >= 0 && s.charAt(i) == ' '){ // 跳过单词间空格
                i--;
            }
            j = i; // j 指向下个单词的尾字符
        }
        return res.toString().trim();
    }
}
```
### 矩阵中的路径
![图示](https://img-blog.csdnimg.cn/7bc1f0013e114046922cb4d1bd6bff53.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    boolean[][] isVisted;
    public boolean exist(char[][] board, String word) {
        isVisted = new boolean[board.length][board[0].length];
        for(int i = 0; i < board.length; i++){
            for(int j = 0; j < board[0].length; j++){
                if(dfs(board, word, i, j, 0)){
                    return true;
                }
            }
        }
        return false;
    }
    public boolean dfs(char[][] board, String word, int i, int j, int index){
        int m = board.length, n = board[0].length;
        if(i >= m || i < 0 || j >= n || j < 0 || isVisted[i][j] || board[i][j] != word.charAt(index)){
                return false;
            }
        if(index == word.length() - 1){
            return true;
        }
        isVisted[i][j] = true;
        boolean res = dfs(board, word, i - 1, j, index + 1) 
                    || dfs(board, word, i, j - 1, index + 1) 
                    || dfs(board, word, i + 1, j, index + 1)
                    || dfs(board, word, i, j + 1, index + 1);
        isVisted[i][j] = false;
        return res;
    }
}
```
### 机器人的运动范围
![图示](https://img-blog.csdnimg.cn/bacdad26181442c597231feff0992c99.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    int res = 0;
    boolean[][] isValid;
    public int movingCount(int m, int n, int k) {
        isValid = new boolean[m][n];
        dfs(0, 0, m, n, k);
        return res;
    }
    public void dfs(int i, int j, int m, int n, int k){
        if(i < 0 || i >= m || j < 0 || j >= n || isValid[i][j] || !isValid(i, j, k)){
            return;
        }
        res++;
        isValid[i][j] = true;
        dfs(i + 1, j, m, n, k);
        dfs(i, j + 1, m, n, k);
        dfs(i - 1, j, m, n, k);
        dfs(i, j - 1, m, n, k);
    }
    public boolean isValid(int i, int j, int k){
        int res = 0;
        while(i != 0){
            res += i % 10;
            i /= 10;
        }
        while(j != 0){
            res += j % 10;
            j /= 10;
        }
        return res <= k;
    }
}
```
### 把数组排成最小的数（排序）
![图示](https://img-blog.csdnimg.cn/9cbbfb24a7fc4e7e9473ca408daaf1d6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
* 思路：
如果拼接字符串后 x + y > y + x，则 x “大于” y ；
反之，如果 x + y < y + x，则 x “小于” y ；
```java
class Solution {
    public String minNumber(int[] nums) {
        String[] strs = new String[nums.length];
        for(int i = 0; i < nums.length; i++){
            strs[i] = nums[i] + "";
        }
        quickSort(strs, 0, nums.length - 1);
        StringBuilder res = new StringBuilder("");
        for(String str : strs){
            System.out.print(str + " ");
            res.append(str);
        }
        return res.toString();
    }
    public void quickSort(String[] strs, int left, int right){
        if(left >= right){
            return;
        }
        int l = left, r = right;
        String val = strs[left];
        while(l < r){
            // l < r && strs[r] >= val
            while(l < r && ((strs[r] + val).compareTo(val + strs[r])) >= 0){
                r--;
            }
            strs[l] = strs[r];
            // l < r && strs[l] < val
            while(l < r && ((strs[l] + val).compareTo(val + strs[l])) < 0){
                l++;
            }
            strs[r] = strs[l];
        }
        strs[l] = val;
        quickSort(strs, left, l - 1);
        quickSort(strs, r + 1, right);
    }
}
```
用内置函数：
```java
class Solution {
    public String minNumber(int[] nums) {
        String[] strs = new String[nums.length];
        for(int i = 0; i < nums.length; i++){
            strs[i] = nums[i] + "";
        }
        Arrays.sort(strs, (a, b) -> (a + b).compareTo(b + a));
        StringBuilder res = new StringBuilder("");
        for(String str : strs){
            System.out.print(str + " ");
            res.append(str);
        }
        return res.toString();
    }
}
```
### 扑克牌中的顺子
![图示](https://img-blog.csdnimg.cn/e6afc1b18b964fa0acf5ef37157d14d5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
集合 Set：
```java
class Solution {
    public boolean isStraight(int[] nums) {
        Set<Integer> set = new HashSet<>();
        int min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
        for(int num : nums){
            if(num == 0){ // 跳过大小王
                continue;
            }
            min = Math.min(min, num); // 得到最小牌
            max = Math.max(max, num); // 得到最大牌
            if(set.contains(num)){ // 若有重复的牌，提前返回 false（0的情况上面已经排除）
                return false;
            }
            set.add(num); // 添加此牌至 Set
        }
        return max - min < 5; // 最大牌 - 最小牌 < 5 则可构成顺子
    }
}
```
排序：
```java
class Solution {
    public boolean isStraight(int[] nums) {
        int joker = 0;
        Arrays.sort(nums);
        for(int i = 0; i < nums.length - 1; i++){
            if(nums[i] == 0){ // 统计大小王的数量
                joker++;
            }else if(nums[i] == nums[i + 1]){ // 若有重复，提前返回 false
                return false;
            }
        }
        return nums[4] - nums[joker] < 5; // 最大牌 - 最小牌 < 5 则可构成顺子
    }
}
```
### 最小的k个数
![图示](https://img-blog.csdnimg.cn/f7357832506e4c9c82c98fcd6e9f0181.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
优先队列：
```java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if(k == 0){
            return new int[0];
        }
        // 默认实现是小顶堆，但需要使用大顶堆
        PriorityQueue<Integer> pq = new PriorityQueue<>((a, b) -> (b - a));
        for(int num : arr){
            if(pq.size() < k){
                pq.offer(num);
            }else if(num < pq.peek()){ // 大顶堆的队头元素是最大的
                pq.poll();
                pq.offer(num);
            }
        }

        int[] nums = new int[k];
        int index = 0;
        for(int num : pq){
            nums[index++] = num;
        }
        return nums;
    }
}
```
### 数据流中的中位数
![图示](https://img-blog.csdnimg.cn/fbba05387027407598d6aeccec66fe44.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
* 思路：
![思路](https://img-blog.csdnimg.cn/55469da84f0e4726b87916bd9795bd1f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_12,color_FFFFFF,t_70,g_se,x_16)
```java
class MedianFinder {
    // 实现小顶堆，存储较大的一半
    PriorityQueue<Integer> pq1 = new PriorityQueue<>();
    // 实现大顶堆，存储较小的一半
    PriorityQueue<Integer> pq2 = new PriorityQueue<>((a, b) -> (b - a));
    
    public MedianFinder() {}
    
    public void addNum(int num) {
        // 当前元素总数为奇数，需向 pq2 添加一个元素
        if(pq1.size() != pq2.size()){
            pq1.add(num); // 将新元素 num 插入至 pq1
            pq2.add(pq1.poll()); // 将 pq1 堆顶元素插入至 pq2
        }else { // 当前元素总数为偶数，需向 pq1 添加一个元素
            pq2.add(num); // 将新元素 num 插入至 pq2
            pq1.add(pq2.poll()); // 将 pq2 堆顶元素插入至 pq1
        }
    }
    
    public double findMedian() {
        return pq1.size() != pq2.size() ? pq1.peek() : (pq1.peek() + pq2.peek()) / 2.0;
    }
}
```
### 平衡二叉树
![图示](https://img-blog.csdnimg.cn/374bb68a34fa466d86b69e00e00a4535.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
方法一：
```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root == null){
            return true;
        }
        return Math.abs(getDepth(root.right) - getDepth(root.left)) <= 1 && isBalanced(root.left) && isBalanced(root.right);
    }
    public int getDepth(TreeNode root){
        if(root == null){
            return 0;
        }
        return Math.max(getDepth(root.left), getDepth(root.right)) + 1;
    }
}
```
方法二：
```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return getHeight(root) >= 0;
    }
    public int getHeight(TreeNode root){
        if(root == null){
            return 0;
        }
        int left = getHeight(root.left);
        int right = getHeight(root.right);
        if(left == -1 || right == -1 || Math.abs(left - right) > 1){
            return -1; // 返回 -1 表示已经不是平衡二叉树了
        }else{
            return Math.max(left, right) + 1;
        }
    }
}
```
### 求1+2+…+n
![图示](https://img-blog.csdnimg.cn/a5c183ac1dad4911998ce6fb1e17a763.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
```java
if(A && B) // 若 A 为 false ，则 B 的判断不会执行（即短路），直接判定 A && B 为 false

if(A || B) // 若 A 为 true ，则 B 的判断不会执行（即短路），直接判定 A || B 为 true
```
```java
class Solution {
    public int sumNums(int n) {
        boolean falg = n > 1 && (n += sumNums(n - 1)) > 0;
        return n;
    }
}
```
### 二叉搜索树的最近公共祖先
![tushi](https://img-blog.csdnimg.cn/6a93bd8202c647d39cdb4e0d3f0705fb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_16,color_FFFFFF,t_70,g_se,x_16)
* 递归（二叉树的最近公共祖先）：
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || root == p || root == q){
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if(left != null && right != null){ // 左右子树分别找到了，说明此时的 root 就是结果
            return root;
        }else if(left != null && right == null){
            return left;
        }else if(left == null && right != null){
            return right;
        }
        return null;
    }
}
```
* 迭代：
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        while(root != null){
            if(root.val > p.val && root.val > q.val){ // p、q都在左子树中
                root = root.left;
            } else if(root.val < p.val && root.val < q.val){ // p、q都在右子树中
                root = root.right;
            } else{
                return root;
            }
        }
        return null;
    }
}
```
### 重建二叉树（分治思想）
![图示](https://img-blog.csdnimg.cn/92419812461b42f091ed5d28dfffaa93.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if(preorder.length == 0){
            return null;
        }
        if(preorder.length == 1){
            return new TreeNode(preorder[0]);
        }
        int index = 0;
        for(int i = 0; i < inorder.length; i++){
            if(inorder[i] == preorder[0]){
                index = i;
                break;
            }
        }
        int[] iLeft = Arrays.copyOfRange(inorder, 0, index);
        int[] iRight = Arrays.copyOfRange(inorder, index + 1, inorder.length);
        int[] pLeft = Arrays.copyOfRange(preorder, 1, iLeft.length + 1);
        int[] pRight = Arrays.copyOfRange(preorder, iLeft.length + 1, preorder.length);
        TreeNode root = new TreeNode(preorder[0]);
        root.left = buildTree(pLeft, iLeft);
        root.right = buildTree(pRight, iRight);
        return root;
    }
}
```
### 数值的整数次方
![图示](https://img-blog.csdnimg.cn/9eff65bef18840b2beae0d8feb501938.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public double myPow(double x, int n) {
        long b = n; // 如果 n 等于 -2147483648，那么 -n 就会越界，所以要用 long 类型
        if(b < 0){
            x = 1 / x;
            b = -b;
        }
        double res = 1;
        while(b > 0){
            if(b % 2 == 1){
                res *= x;
            }
            b /= 2;
            x *= x;
        }
        return res;
    }
}
```
### 二叉搜索树的后序遍历序列
![图示](https://img-blog.csdnimg.cn/0609a33cb795469a9f5c4f2bdc2200aa.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public boolean verifyPostorder(int[] postorder) {
        return judge(postorder, 0, postorder.length - 1);
    }
    public boolean judge(int[] postorder, int left, int right){
        if(left >= right){
            return true;
        }
        int root = postorder[right]; // 后续遍历的最后一个结点是根结点
        int mid = left;
        while(postorder[mid] < root){ // 二叉搜索树的右子树比根结点都要小
            mid++;
        }
        /* [left, mid) 中的数组值都比根结点 root 小，我们还需要确定 [mid, right) 中的数组值
        是否都比根结点 root 大，如果 [mid, right) 的数组值中有比根结点小的值则直接返回 false */
        int temp = mid;
        while(temp < right){
            if(postorder[temp] < root){
                return false;
            }
            temp++;
        }
        return judge(postorder, left, mid - 1) && judge(postorder, mid, right - 1);
    }
}
```
### 不用加减乘除做加法
![图示](https://img-blog.csdnimg.cn/b6611885098b4b01ae5aa1ad26dbe029.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public int add(int a, int b) {
        while(b != 0){ // 当进位为 0 时跳出
            int temp = (a & b) << 1; // temp 记录进位的值（只有 1+1 时会产生进位）
            a = a ^ b; // a 记录非进位的和（相当于把 1+1 看成 0+0，然后再计算相加的和）
            b = temp; // b 记录进位的值
        }
        return a;
    }
}
```
### 数组中数字出现的次数
![图示](https://img-blog.csdnimg.cn/cb3aa7accf2744ec8aa9b343457b93ef.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public int[] singleNumbers(int[] nums) {
        // 先对所有数字进行一次异或，得到两个出现一次的数字的异或值。
        int n = 0;
        for(int num : nums){
            n = n ^ num;
        }
        // 在异或结果中找到任意为 1 的位。这里为了方便，选取的是不为 0 的最低位。也可以选择其它不为 0 的位置
        int m = 1;
        while((n & m) == 0){
            m <<= 1;
        }
        // 根据这一位对所有的数字进行分组：两个只出现一次的数字在不同的组中；相同的数字会被分到相同的组中。
        int x = 0, y = 0;
        for(int num : nums){
            if((num & m) != 0){
                x ^= num;
            }else{
                y ^= num;
            }
        }
        return new int[]{x, y};
    }
}
```
### 数组中出现次数超过一半的数字
![图示](https://img-blog.csdnimg.cn/c583b28cf5b94d9083547c6471137f94.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
* 摩尔投票算法：设输入数组 nums 的众数为 x ，数组长度为 n 。
推论一：若记众数的票数为 +1，非众数的票数为 -1 ，则一定有所有数字的票数和 > 0 
推论二：若数组的前 a 个数字的票数和 = 0 ，则数组剩余 (n−a) 个数字的票数和一定仍 > 0 ，即后 (n−a) 个数字的众数仍为 x
```java
class Solution {
    public int majorityElement(int[] nums) {
        int temp = nums[0]; // 假设第一个元素就是众数
        int count = 1;
        for(int i = 1; i < nums.length; i++){
            if(temp == nums[i]){
                count++;
            }else{
                count--;
            }
            if(count == 0){
                temp = nums[i];
                count = 1;
            }
        }
        return temp;
    }
}
```
### 构建乘积数组
![图示](https://img-blog.csdnimg.cn/0c61c11cd2134a77834ff31680a03429.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public int[] constructArr(int[] nums) {
        if(nums.length == 0) return new int[0];
        // 乘积 = 当前数左边的乘积 * 当前数右边的乘积
        int[] left = new int[nums.length];
        left[0] = 1;
        for(int i = 1; i < nums.length; i++){
            left[i] = nums[i - 1] * left[i - 1];
        }
        int[] right = new int[nums.length];
        right[nums.length - 1] = 1;
        for(int i = nums.length - 2; i >= 0; i--){
            right[i] = nums[i + 1] * right[i + 1];
        }
        int[] res = new int[nums.length];
        for(int i = 0; i < nums.length; i++){
            res[i] = left[i] * right[i];
        }
        return res;
    }
}
```
```java
class Solution {
    public int[] constructArr(int[] nums) {
        if(nums.length == 0) return new int[0];
        int[] res = new int[nums.length];
        // 乘积 = 当前数左边的乘积 * 当前数右边的乘积
        int p = 1;
        for(int i = 0; i < nums.length; i++){
            res[i] = p;
            p *= nums[i]; // 此时 res 数组存储的是除去当前元素左边元素的乘积
        }
        int q = 1;
        for(int i = nums.length - 1; i >= 0; i--){
            res[i] *= q;
            q *= nums[i];
        }
        return res;
    }
}
```
### 顺时针打印矩阵
![图示](https://img-blog.csdnimg.cn/5d352750f5974176983f6dd28038d65c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public int[] spiralOrder(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return new int[0];
        }
        int m = matrix.length, n = matrix[0].length;
        int left = 0, right = n - 1, top = 0, bottom = m - 1;
        int[] res = new int[m * n];
        int index = 0;
        while(true){
            for(int i = left; i <= right; i++){ // left to right
                res[index++] = matrix[top][i];
            }
            top++;
            if(top > bottom) break;

            for(int i = top; i <= bottom; i++){ // top to bottom
                res[index++] = matrix[i][right];
            }
            right--;
            if(left > right) break;

            for(int i = right; i >= left; i--){ // right to left
                res[index++] = matrix[bottom][i];
            }
            bottom--;
            if(top > bottom) break;

            for(int i = bottom; i >= top; i--){ // bottom to top
                res[index++] = matrix[i][left];
            }
            left++;
            if(left > right) break;
        }
        return res;
    }
}
```
### 栈的压入、弹出序列
![图示](https://img-blog.csdnimg.cn/c3d18abdf83e46b2ae4a0fd99a205c3e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        Stack<Integer> stack = new Stack<>(); // 辅助栈
        int index = 0;
        for(int i = 0; i < pushed.length; i++){
            stack.push(pushed[i]); // 入栈
            while(!stack.isEmpty() && stack.peek() == popped[index]){
                stack.pop();
                index++;
            }
        }
        return stack.isEmpty();
    }
}
```
### 圆圈中最后剩下的数字
![图示](https://img-blog.csdnimg.cn/0ff179f3886945e695a0a207826dfa91.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
* 如果采用模拟链表的方式，时间复杂度是 O(mn)。因为每次找到要删除的那个数字，需要 O(m) 的时间复杂度，然后删除了 n−1 次。
* 最终只剩下一个人时的安全位置肯定为 0，反推人数为 n 时的安全位置：
人数为1： 0
人数为2： (0 + m) % 2
人数为3： ((0 + m) % 2 + m) % 3
人数为4： (((0 + m) % 2 + m) % 3 + m) % 4
......
迭代推理到 n 就可以得出答案
```java
class Solution {
    public int lastRemaining(int n, int m) {
        return f(n, m);
    }
    public int f(int n, int m){
        if(n == 1){ // 当只剩下一个人的时候，其编号一定为 0
             return 0;
        }
        int x = f(n - 1, m);
        return (x + m) % n;
    }
}
```
```java
class Solution {
    public int lastRemaining(int n, int m) {
        int x = 0; // 当只剩下一个人的时候，其编号一定为 0
        for(int i = 2; i <= n; i++){
            x = (x + m) % i;
        }
        return x;
    }
}
```
### 表示数值的字符串
![图示](https://img-blog.csdnimg.cn/446810143c684f2cae3357389e6217fa.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_13,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public boolean isNumber(String s) {
        s = s.trim(); // 去除字符串首尾的空格
        if(s.length() == 0) return false;
        boolean hasE = false; // 是否包含 e 或者 E
        boolean hasNum = false; // 是否包含数
        boolean hasDot = false; // 是否包含小数点
        for(int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            if(c >= 48 && c <= 57){
                hasNum = true;
            }else if(c == 'e' || c == 'E'){
                if(hasE || !hasNum){ // e/E 之前不能出现 e/E，必须出现数
                    return false;
                }
                hasE = true;
                hasNum = false; // 重置 hasNum，确保 e/E 后面也能有数
            }else if(c == '.'){
                if(hasE || hasDot){ // .之前不能出现 . 或者 e/E
                    return false;
                }
                hasDot = true;
            }else if(c == '+' || c == '-'){
                // +/- 必须出现在开头的位置 或者 e/E 后面的第一个位置才合法
                if(i != 0 && s.charAt(i - 1) != 'e' && s.charAt(i - 1) != 'E'){
                    return false;
                }
            }else{ // 其它不合法字符
                return false;
            }
        }
        return hasNum;
    }
}
```
### 序列化二叉树
![图示](https://img-blog.csdnimg.cn/51e246e13a604ed091a712813189a6fc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
```java
public class Codec {
    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if(root == null){
            return "[]";
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        StringBuilder sb = new StringBuilder("[");
        while(!queue.isEmpty()){
            int size =  queue.size();
            for(int i = 0; i < size; i++){
                TreeNode temp = queue.poll();
                if(temp != null){
                    queue.add(temp.left);
                    queue.add(temp.right);
                    sb.append(temp.val + ",");
                }else{
                    sb.append("null,");
                }
            }
        }
        sb.deleteCharAt(sb.length() - 1); // 删除最后一个多余的逗号
        sb.append("]");
        return sb.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if(data.equals("[]")){
            return null;
        }
        String[] vals = data.substring(1, data.length() - 1).split(",");
        TreeNode root = new TreeNode(Integer.parseInt(vals[0]));
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        int left = 1, right = 2; // 记录当前结点的左右结点的下标
        while(!queue.isEmpty()){
            TreeNode temp = queue.poll();
            if(vals[left].equals("null")){
                temp.left = null;
            }else{
                temp.left = new TreeNode(Integer.parseInt(vals[left]));
                queue.add(temp.left);
            }
            left += 2;
            if(vals[right].equals("null")){
                temp.right = null;
            }else{
                temp.right = new TreeNode(Integer.parseInt(vals[right]));
                queue.add(temp.right);
            }
            right += 2;
            // if(left >= vals.length || right >= vals.length){
                // break;
            // }
        }
        return root;
    }
}
```
```java
public class Codec {
    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if(root == null){
            return "[]";
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        StringBuilder sb = new StringBuilder("[");
        while(!queue.isEmpty()){
            int size =  queue.size();
            for(int i = 0; i < size; i++){
                TreeNode temp = queue.poll();
                if(temp != null){
                    queue.add(temp.left);
                    queue.add(temp.right);
                    sb.append(temp.val + ",");
                }else{
                    sb.append("null,");
                }
            }
        }
        sb.deleteCharAt(sb.length() - 1); // 删除最后一个多余的逗号
        sb.append("]");
        return sb.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if(data.equals("[]")){
            return null;
        }
        String[] vals = data.substring(1, data.length() - 1).split(",");
        TreeNode root = new TreeNode(Integer.parseInt(vals[0]));
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        int i = 1; // 记录当前结点的子结点
        while(!queue.isEmpty()){
            TreeNode temp = queue.poll();
            String left = vals[i++]; // 当前结点的左侧子结点的值
            if(left.equals("null")){
                temp.left = null;
            }else{
                temp.left = new TreeNode(Integer.parseInt(left));
                queue.add(temp.left);
            }
            String right = vals[i++]; // 当前结点的右侧子结点的值
            if(right.equals("null")){
                temp.right = null;
            }else{
                temp.right = new TreeNode(Integer.parseInt(right));
                queue.add(temp.right);
            }
        }
        return root;
    }
}
```
### 数组中的逆序对
![图示](https://img-blog.csdnimg.cn/0ccc462fdc2c40f1821b3e13a3f3fb82.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    int res = 0;
    public int reversePairs(int[] nums) {
        mergeSort(nums, 0, nums.length - 1);
        return res;
    }
    public void mergeSort(int[] nums, int left, int right){
        if(left < right){
            int mid = left + (right - left) / 2;
            mergeSort(nums, left, mid);
            mergeSort(nums, mid + 1, right);
            merge(nums, left, mid, right);
        }
    }
    public void merge(int[] nums, int left, int mid, int right){
        int i = left;
        int j = mid + 1;
        int[] temp = new int[right - left + 1];
        int t = 0;
        while(i <= mid && j <= right){
            if(nums[i] <= nums[j]){
                temp[t++] = nums[i++];
            }else{
                temp[t++] = nums[j++];
                res += mid - i + 1;
            }
        }
        while(i <= mid){
            temp[t++] = nums[i++];
        }
        while(j <= right){
            temp[t++] = nums[j++];
        }
        t = 0;
        while(t < temp.length){
            nums[left++] = temp[t++];
        }
    }
}
```
### 回文链表
![图示](https://img-blog.csdnimg.cn/6f776d152153412596e2a599c5ec3733.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        if(head == null || head.next == null){
            return true;
        }
        ListNode slow = head;
        ListNode fast = head;
        // slow 最终指向中间结点
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode newHead = reverse(slow);
        // 不能只写 head != null，因为这样的话奇数个结点没问题，但偶数个结点会有问题
        while(newHead != null){
            if(head.val != newHead.val){
                return false;
            }
            head = head.next;
            newHead = newHead.next;
        }
        return true;
    }
    // 反转链表
    public ListNode reverse(ListNode head){
        ListNode pre = null;
        ListNode cur = head;
        while(cur != null){
            ListNode temp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = temp;
        }
        return pre;
    }
}
```
### K 个一组翻转链表
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
递归：
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
### 正则表达式匹配
![图示](https://img-blog.csdnimg.cn/163aa59386084da481c21eb984395dca.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
* 注意：保证每次出现字符 * 时，前面都匹配到有效的字符

```java
class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length(), n = p.length();
        boolean[][] dp = new boolean[m + 1][n + 1]; // s 的前 i 个字符和 p 的前 j 个字符是否匹配
        // 如果 s 和 p 都为空，则肯定匹配
        dp[0][0] = true;
        // 如果 s 不为空，p 为空，则肯定匹配不成功
        // 如果 s 为空，p 不为空
        for(int j = 2; j <= n; j++){
            // 此时比如：c*a*b* 能够和 "" 匹配。即 p 的偶数位为 * 时才能够匹配
            if(p.charAt(j - 1) == '*'){
                dp[0][j] = dp[0][j - 2];
            }
        }

        for(int i = 1; i <= m; i++){
            for(int j = 1; j <= n; j++){
                // 如果 s 的第 i 个元素和 p 的第 j 个元素相等，或者 p 的第 j 个元素是 .
                if(s.charAt(i - 1) == p.charAt(j - 1) || p.charAt(j - 1) == '.'){
                    dp[i][j] = dp[i - 1][j - 1];
                }
                // 如果 p 的第 j 个元素是 *
                if(p.charAt(j - 1) == '*'){
                    // 如果 s 的第 i 个元素和 p 的第 j-1 个元素相等，或者 p 的第 j-1 个元素是 .
                    if(s.charAt(i - 1) == p.charAt(j - 2) || p.charAt(j - 2) == '.'){
                        dp[i][j] = dp[i][j - 2] || dp[i - 1][j];
                    }else{
                        dp[i][j] = dp[i][j - 2];
                    }
                }
            }
        }
        return dp[m][n];
    }
}
```
### 替换空格
在 Java 中，字符串被设计成「不可变」的类型，即无法直接修改字符串的某一位字符，需要新建一个字符串实现。
```java
class Solution {
    public String replaceSpace(String s) {
        StringBuilder res = new StringBuilder("");
        for(int i = 0; i < s.length(); i++){
            if(' ' == s.charAt(i)){
                res.append("%20");
            }else {
                res.append(s.charAt(i));
            }
        }
        return res.toString();
    }
}
```