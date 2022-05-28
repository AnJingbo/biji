

break和continue关键字只能中断当前循环，但若随同**标签**一起使用，它们就会中断循环，直到标签所在的地方。

例如：
```java
public class ShiYan7 {
    public static void main(String[] args) {
    // 带标签的break会中断并跳出标签所指的循环
        ok:
        for(int i = 0; i < 10; i++){
            for(int j = 0; j < 10; j++){
                System.out.println("i = "  + i + ", j = " + j);
                break ok;
            }
        }
        
        System.out.println("============");
        
        // 带标签的continue会到达标签的位置，并重新进入紧接在那个标签后面的循环
        ok:
        for(int i = 0; i < 10; i++){
            for(int j = 0; j < 10; j++){
                System.out.println("i = "  + i + ", j = " + j);
                continue ok;
            }
        }
    }
}
```
执行结果：
```java
i = 0, j = 0
============
i = 0, j = 0
i = 1, j = 0
i = 2, j = 0
i = 3, j = 0
i = 4, j = 0
i = 5, j = 0
i = 6, j = 0
i = 7, j = 0
i = 8, j = 0
i = 9, j = 0
```