

![图示](https://img-blog.csdnimg.cn/757136787ee948eba8c54327de3df8da.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
* 哈希链表：双向链表和哈希表的结合体：
![图示](https://img-blog.csdnimg.cn/c2d1fd94671b48ef98dfd1c11b46838c.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
注：图来自 labuladong
```java
/*
 * 最近最少使用算法(Least Recently Used，LRU)
 *     计算机的缓存容量有限，如果缓存满了就需要删除一些内容。
 *     LRU 就是认为最近使用过的数据是有用的，优先淘汰一段时间内没有使用的字块。
 *     实现注意事项：
 *         （1）cache 中的元素必须有时序，以区分最近使用和最久未使用的数据。
 *         （2）当容量满了之后要删除最久未使用的那个元素。
 *         （3）每次访问 cache 中的某个 key，需要将这个元素变为最近使用的。
 *         （4）如果我们每次默认从链表尾部添加元素，那么越靠近尾部的元素就是最近使用的，
 *             越靠近头部的元素就是最久未使用的。
 * */
class LRUCache {
    LinkedHashMap<Integer, Integer> cache = new LinkedHashMap<>();
    int capacity; // 缓存的最大容量
    public LRUCache(int capacity) {
        this.capacity = capacity;
    }
    
    public int get(int key) {
        if(!cache.containsKey(key)){
            return -1;
        }
        // 将 key 变为最近使用
        makeRecently(key);
        return cache.get(key);
    }
    
    public void put(int key, int val) {
        if(cache.containsKey(key)){
            // 修改 key 的值
            cache.put(key, val);
            // 将 key 变为最近使用
            makeRecently(key);
            return;
        }
            
        if(cache.size() == capacity){
            // 链表头部就是最久未使用的 key
            int oldestKey = cache.keySet().iterator().next();
            cache.remove(oldestKey);
        }
        // 将新的 key 添加到链表尾部（链表尾部就是最近使用的 key）
        cache.put(key, val);      
    }

    public void makeRecently(int key){
        int val = cache.get(key);
        // 删除 key，重新加入到队尾
        cache.remove(key);
        cache.put(key, val);
    }
}
```
手动实现 LRU，不借助 Java 内置的 LinkedHashMap：
```java
/*
 * 最近最少使用算法(Least Recently Used，LRU)
 *     计算机的缓存容量有限，如果缓存满了就需要删除一些内容。
 *     LRU 就是认为最近使用过的数据是有用的，优先淘汰一段时间内没有使用的字块。
 *     实现注意事项：
 *         （1）cache 中的元素必须有时序，以区分最近使用和最久未使用的数据。
 *         （2）当容量满了之后要删除最久未使用的那个元素。
 *         （3）每次访问 cache 中的某个 key，需要将这个元素变为最近使用的。
 *         （4）如果我们每次默认从链表尾部添加元素，那么越靠近尾部的元素就是最近使用的，
 *             越靠近头部的元素就是最久未使用的。
 * */
class Node{
    int key;
    int val;
    Node next;
    Node pre;
    public Node(int key, int val){
        this.key = key;
        this.val = val;
    }
}

class DoubleList{
    // 虚拟头结点
    private Node head = new Node(0, 0);
    // 虚拟尾节点
    private Node tail = new Node(0, 0);
    // 记录链表的长度
    private int size;
    public DoubleList(){
        head.next = tail;
        tail.pre = head;
    }
    // 在链表末尾加入结点 node
    public void addLast(Node node){
        Node temp = tail.pre;
        temp.next = node;
        node.pre = temp;
        node.next = tail;
        tail.pre = node;
        size++;
    }
    // 删除链表中的结点 node（该结点一定存在）
    public void remove(Node node){
        // 因为有虚拟头结点和虚拟尾结点，所以可以不考虑空指针直接像下面这样写
        node.pre.next = node.next;
        node.next.pre = node.pre;
        size--;
    }
    // 删除链表中的第一个结点，并返回该结点
    public Node removeFirst(){
        if(head.next == tail){
            return null;
        }
        Node first = head.next;
        remove(first);
        return first;
    }
    // 返回链表的长度
    public int size(){
        return size;
    }
}
class LRUCache{
    // HashMap 中的 key 的值就是 cache 链表中的结点
    private HashMap<Integer, Node> map = new HashMap<>();
    private DoubleList cache = new DoubleList();
    int capacity;
    public LRUCache(int capacity){
        this.capacity = capacity;
    }
    public int get(int key){
        if(!map.containsKey(key)){
            return -1;
        }
        Node node = map.get(key);
        makeRecently(key);
        return node.val;
    }
    public void put(int key, int val){
        Node node = new Node(key, val);
        if(map.containsKey(key)){ // 更新 key 的值
            Node temp = map.get(key);
            // 从 map 中删除这个值并进行更新
            map.remove(key);
            map.put(key, node);
            // 从链表中删除这个值并进行更新
            cache.remove(temp);
            cache.addLast(node);
            return;
        }
        if(capacity == map.size()){
            // 删除链表中的第一个结点，即最久未使用的那个结点
            Node first = cache.removeFirst();
            // 删除 map 中对应的结点
            map.remove(first.key);
        }
        map.put(key, node);
        cache.addLast(node);
    }
    public void makeRecently(int key){
        Node node = map.get(key);
        // 先从链表中删除这个结点
        cache.remove(node);
        // 重新插入到队尾
        cache.addLast(node);
    }
}
```
* 由手动实现的步骤可知：
（1）必须使用双向链表。因为我们需要删除操作，只有双向链表才能保证操作的时间复杂度是 O(1)。
（2）双向链表中的结点必须同时存储 key 和 val 的值。注意在 put() 方法中，如果缓存的容量已满，我们不仅仅需要删除链表的最后一个结点 node，还需要使用这个 node 结点中的 key 将 map 中对应的结点删除，因此必须要有 key。如果 Node 结构中没有 key，那么我们就无法得知 key 是什么，就无法删除 map 中的键，造成错误。