

```java
public class HashMapSort {
    public static void main(String[] args) {
        HashMap<Integer, Integer> map = new HashMap<>();
        map.put(6, 2);
        map.put(10, 1);
        map.put(3, 4);
        map.put(2, 8);
        map.put(19, 6);
        map.put(26, 4);
        map.put(15, 5);
        for(Map.Entry<Integer, Integer> entry : map.entrySet()){
            System.out.println(entry.getKey() + " = " + entry.getValue());
        }
        System.out.println("-----------------------------");

        // 将 map.entrySet() 转换成 list
        ArrayList<Map.Entry<Integer, Integer>> list = new ArrayList<>(map.entrySet());
        Collections.sort(list, new Comparator<Map.Entry<Integer, Integer>>() {
            @Override
            public int compare(Map.Entry<Integer, Integer> o1, Map.Entry<Integer, Integer> o2) {
                return o1.getValue() - o2.getValue();
            }
        });
        for(Map.Entry<Integer, Integer> entry : list){
            System.out.println(entry.getKey() + " = " + entry.getValue());
        }
    }
}
```
运行结果：
![图示](https://img-blog.csdnimg.cn/20210603082927205.png)