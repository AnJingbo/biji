

### ThreadLocal 基础知识
* ThreadLocal 作用：在多线程并发场景下，ThreadLocal 为每个线程都提供了一个变量的副本，使得每个线程中的变量都是相互独立的，不会互相影响，避免了当前线程的变量被其他线程所修改
* ThreadLocal 常用方法：
（1）set()：将变量绑定到当前线程中
（2）get()：获取当前线程绑定的变量
* ThreadLocal 的设计：
（1）每个 Thread 线程内部都有一个 Map（ThreadLocalMap）
（2）Map 里面存储 ThreadLocal 对象（key）和线程的变量副本（value）
（3）Thread 内部的 Map 是由 ThreadLocal 维护的，由 ThreadLocal 负责向 Map 中获取和设置线程的变量值
（4）对于不同的线程，每次获取副本值时，别的线程并不能获取到当前线程的副本值，形成了副本的隔离，互不干扰
### ThreadLocal 的 key 使用了强引用，会造成内存泄漏吗
![ts](https://img-blog.csdnimg.cn/d387a992f613474e8d0c8f430e36f13b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
 1. 假设业务代码中使用完 ThreadLocal，threadLocal Ref 被回收了
2. 因为 ThreadLocalMap 的 Entry 持有 ThreadLocal 的强引用，所以会造成 ThreadLocal 无法被 GC 回收
3. 但是在没有手动删除这个 Entry 以及 CurrentThread 依然运行的前提下，始终存在有强引用链：threadRef -> currentThread -> threadLocalMap -> Entry，Entry 就不会被回收（Entry 包括了 ThreadLocal 实例和 value），导致 Entry 内存泄露。

也就是说，ThreadLocalMap 中的 key 使用了强引用，有可能存在内存泄漏
### ThreadLocal 的 key 使用了弱引用，会造成内存泄漏吗
![ts](https://img-blog.csdnimg.cn/9111be7ec48d46b78ef2668fb8f671ba.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
1. 假设业务代码中使用完 ThreadLocal，threadLocal Ref 被回收了
2. 由于 ThreadLocalMap 只持有 ThreadLocal 的弱引用，没有任何强引用指向 ThreadLocal 实例，所以 ThreadLocal 就可以顺利地被 GC 回收，此时 Entry 中的 key 为 null
3. 但是在没有手动删除这个 Entry 以及 CurrentThread 依然运行的前提下，也存在有强引用链：threadRef -> currentThread -> threadLocalMap -> Entry -> value，value 不会被回收，但这块 value 永远不会被访问到了，导致 value 内存泄露。

也就是说，ThreadLocalMap 中的 key 使用了弱引用，也有可能存在内存泄漏
### 内存泄漏的原因
* 以上两种内存泄漏的情况中，都有两个前提：
  （1）没有手动删除 Entry
  （2）CurrentThread 依然运行
* 分析：
（1）第一点：只要使用完 ThreadLocal，调用其 remove() 方法删除对应的 Entry 就能避免内存泄漏
（2）第二点：由于 ThreadLocalMap 是 Thread 的一个属性，被当前线程所引用，所以它的生命周期和 Thread 一样长。那么当 ThreadLocal 在使用完后，如果当前 Thread 也随之结束，ThreadLocalMap 也自然会随之被 GC 回收，从根源上避免了内存泄漏
* 避免内存泄漏有两种方式：
  （1）**使用完 ThreadLocal，调用其 remove() 方法删除对应的 Entry**
  （2）使用完 ThreadLocal，当前线程 currentThread 也随之运行结束

相比于第一种方式，第二种显然不好控制，尤其是在使用线程池时，线程结束是不会销毁的

### 既然强引用和弱引用都会导致内存泄漏，那么为什么要使用弱引用
* 事实上，在 ThreadLocalMap 中的 set 和 getEntry 方法中，会对 key 为 null 进行判断，如果 key 为 null（即 ThreadLocal  为 null），那么会对 value 也置 0。这就意味着使用完 ThreadLocal、CurrentThread，就算忘记调用 remove 方法，弱引用也比强引用多一层保障：弱引用的 ThreadLocal 会被回收，对应的 value 在下一次 ThreadLocalMap 调用 set、get、remove 中的任一方法的时候会被清除，从而避免内存泄漏
### ThreadLocalMap
* ThreadLocalMap 时 ThreadLocal 的内部类，没有实现 Map 接口，以独立的方式实现了 Map 的功能，其内部的 Entry 也是独立实现的，``Entry extends WeakReference<ThreadLocal<?>>``，即 Entry 的 key 是弱引用类型。（ThreadLocalMap 底层数组：Entry[] table）
* **ThreadLocalMap 是由数组实现的，没有链表结构，当发生哈希冲突时采用的是线性探测法来解决冲突。** 该方法一次探测下一个地址，直到有空的地址后插入。比如：假设当前数组 table 的长度为 16，计算出来 key 的下标为 14，如果 table[14] 上已经有值，并且其 key 与当前的 key 不一致，那么就发生了 hash 冲突，这个时候会将 14 加 1 得到 15，取 table[15] 进行判断，如果还是发生哈希冲突则会回到 0，取 table[0]，以此类推，直到可以插入
* ThreadLocalMap 的初始容量为 16，扩容的阈值是：数组长度的 2/3。
### 实例
```java
public class ThreadLocalTest {
    static String str;
    public static void main(String[] args) {
        ThreadLocal threadLocal = new ThreadLocal();

        for(int i = 1; i <= 5; i++){
            final int temp = i;
            new Thread(() -> {
                // str = Thread.currentThread().getName() + "的数据";
                threadLocal.set(Thread.currentThread().getName() + "的数据");
                try {
                    TimeUnit.SECONDS.sleep(1);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                // System.out.println(Thread.currentThread().getName() + " -> " + str);
                System.out.println(Thread.currentThread().getName() + " -> " + threadLocal.get());
            }, "线程 " + temp).start();
        }
    }
}
```