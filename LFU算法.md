

![图示](https://img-blog.csdnimg.cn/941a4732c0014f94af59adb567bd7d3c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
```java
/*
* LFU 算法：淘汰访问频次最低的元素，如果访问频次最低的数据有多条，则需要淘汰最旧的数据。
* */
class LFUCache {
    // 存放 key 到 val 的映射
    HashMap<Integer, Integer> keyToVal = new HashMap<>();
    // 存放 key 到 使用频次freq 的映射
    HashMap<Integer, Integer> keyToFreq = new HashMap<>();
    // 使用频次freq 到 key 列表的映射。用于删除最久未使用的 key（即链表头的 key）
    HashMap<Integer, LinkedHashSet<Integer>> freqToKeys = new HashMap<>();
    // 记录最小的频次
    int minFreq = 0;
    // 记录 LFU 缓存的最大容量
    int capacity;
    public LFUCache(int capacity){
        this.capacity = capacity;
    }
    public int get(int key){
        // 如果不存在 key
        if(!keyToVal.containsKey(key)){
            return -1;
        }
        // 增加 key 对应的 freq
        increaseFreq(key);
        return keyToVal.get(key);
    }
    public void put(int key, int val){
        if(capacity <= 0){
            return;
        }
        // 如果 key 已经存在，那么修改对应的 value 即可
        if(keyToVal.containsKey(key)){
            keyToVal.put(key, val);
            // key 对应的 freq 加 1
            increaseFreq(key);
            return;
        }
        // 如果 key 不存在，需要插入
        // 容量已满的话需要淘汰一个 freq 最小的 key
        if(capacity == keyToVal.size()){
            removeMinFreqKey();
        }

        // 插入 key 和 value，对应的 freq 为 1
        keyToVal.put(key, val);
        keyToFreq.put(key, 1);
        freqToKeys.putIfAbsent(1, new LinkedHashSet<>());
        freqToKeys.get(1).add(key);
        // 插入新的 key 后最小的 freq 肯定是 1
        minFreq = 1;
    }
    // 增加 key 对应的 freq
    public void increaseFreq(int key){
        int freq = keyToFreq.get(key);
        // 更新 keyToFreq 表。使用频次 + 1
        keyToFreq.put(key, freq + 1);
        // 更新 freqToKeys 表。
        // 将 key 从 freq 对应的列表中删除
        freqToKeys.get(freq).remove(key);
        // 将 key 加入 freq+1 对应的列表中
        freqToKeys.putIfAbsent(freq + 1, new LinkedHashSet<>());
        freqToKeys.get(freq + 1).add(key);
        // 如果 freq 对应的列表空了，移除这个 freq
        if(freqToKeys.get(freq).isEmpty()){
            freqToKeys.remove(freq);
            // 如果这个 freq 恰好是 minFreq，更新 minFreq
            if(freq == minFreq){
                minFreq++;
            }
        }
    }
    // 淘汰一个 freq 最小的 key
    public void removeMinFreqKey(){
        // 得到 freq 最小时对应的 key 列表
        LinkedHashSet<Integer> keyList = freqToKeys.get(minFreq);
        // 最先被插入的那个 key（也就是最长时间没有使用过的 key，即链表头部的 key）就是该被淘汰的 key
        int deleteKey = keyList.iterator().next();
        keyList.remove(deleteKey);
        if(keyList.isEmpty()){
            freqToKeys.remove(minFreq);
            freqToKeys.remove(minFreq);
            // 如果freq 对应的 key 列表空了，此时需要移除这个 freq，因为此时的 freq 就是 minFreq，因此 minFreq 需要加 1。
            // 但其实没有下面这行代码也可以。因为只有在 put 方法插入 key 的时候才有可能调用 removeMinFreqKey() 方法，而在 put() 方法
            // 中，插入新的 key 时一定会把 minFreq 更新变成 1，因此说即使这里 minFreq 变了，我们也不需要管它。
            minFreq++;
        }
        keyToVal.remove(deleteKey);
        keyToFreq.remove(deleteKey);
    }
}
```