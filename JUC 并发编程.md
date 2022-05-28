

> 什么是 JUC？JUC 是 java.util.concurrent 工具包的简称
## 基础知识
```java
public class LambdaTest {
    public static void main(String[] args) {
        /*for(int i = 1; i <= 10; i++){
            new Thread(() -> {
                // 注：在 lambda 里面是里是拿不到 i 的
                System.out.println(i); // 会报错
            }).start();
        }*/
        for(int i = 1; i <= 10; i++){
            final int temp = i; // 必须要有 final。在 jdk8 以后，可以不写 final ，但是会默认加上 final。在 lambda 里面是修改不了 temp 的值的
            new Thread(() -> {
                System.out.println(temp);
            }).start();
        }
    }
}
```
* 实现线程的三种方式：
（1）创建一个类继承 Thread 类，然后重写 run() 方法，在使用的时候直接 new 新建的类即可。
（2）创建一个类实现 Runnable 接口，然后重写 run() 方法，在使用的时候需要 new 一个 Thread()，并将实现了 Runnable 接口的那个类作为参数传进去。
（3）创建一个类实现 Callable 接口，然后重写 call() 方法，然后再 new 一个 FutureTask()，并将实现了 Callable 接口的哪个类作为参数传进去，然后再 new 一个 Thread()，并将新建的 FutureTask 对象作为参数传进去。注：FutureTask 类实现了 Runnable 接口。
* 注：以下两种写法作用相同（lambda 表达式）：
```java
new Thread(new Runnable() {
    public void run() {
        for (int i = 0; i < 10000; i++) {
            System.out.println(Thread.currentThread().getName() + " -> " + i);
        }
    }
}).start();

new Thread(() -> {
    for (int i = 0; i < 10000; i++) {
        System.out.println(Thread.currentThread().getName() + " -> " + i);
    }
}).start();
```
## 一、生产者消费者问题
### 1.1 基础知识
* o.wait() 方法：让正在 o 对象上活动的线程进入等待，并且释放该线程占有的锁。
* o.notiry() 方法：让正在 o 对象上等待的某个线程唤醒。
* o.notifyAll() 方法：让正在 o 对象上等待的所有线程唤醒。
* 注：线程被唤醒以后会从上次等待的位置继续往下执行，也就是会接着执行 wait() 方法之后的代码。
* Conditon 接口中的 await() 对应 Object 中的 wait()；
Condition 接口中的 signal() 对应 Object 中的 notify()；
Condition 接口中的 signalAll() 对应 Object 中的 notifyAll()。
### 1.2 Synchronized 版本的生产者消费者问题
```java
public class Test7 {
    public static void main(String[] args) {
        Data data = new Data();
        new Thread(() ->{
            for(int i = 0; i < 10; i++){
                data.produce();
            }
        }, "A").start();
        new Thread(() ->{
            for(int i = 0; i < 10; i++){
                data.consume();
            }
        }, "B").start();
        new Thread(() ->{
            for(int i = 0; i < 10; i++){
                data.produce();
            }
        }, "C").start();
        new Thread(() ->{
            for(int i = 0; i < 10; i++){
                data.consume();
            }
        }, "D").start();
    }
}
class Data{
    int num = 0;
    // 步骤：判断等待、业务、通知唤醒
    public synchronized void produce(){
        // 线程被唤醒之后是会从上次等待的位置继续往下执行，也就是会接着执行 wait() 方法之后的代码
        // 所以假设 A、C 线程同时被唤醒以后，如果是 if，那么不会再次进行判断，A、C线程下一条要执行的语句都是下面的 num++，所以会出现 2 的情况。而如果是 while，那么会再次执行判断，此时只能有一个线程执行到 num ++ 语句
        while(num != 0){ // 不能是 if
            try {
                this.wait(); // 当前线程进入等待状态，并且释放 data 对象的锁
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        num++;
        System.out.println(Thread.currentThread().getName() + " -> " + num);
        this.notifyAll();
    }
    // 步骤：判断等待、业务、通知唤醒
    public synchronized void consume(){
        while(num == 0){ // 不能是 if
            try {
                this.wait(); // 当前线程进入等待状态，并且释放 data 对象的锁
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        num--;
        System.out.println(Thread.currentThread().getName() + " -> " + num);
        this.notifyAll();
    }
}
```
### 1.3 Lock 版本的生产者消费者问题
```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Test8 {
    public static void main(String[] args) {
        Data2 data = new Data2();
        new Thread(() ->{
            for(int i = 0; i < 10; i++){
                data.produce();
            }
        }, "A").start();
        new Thread(() ->{
            for(int i = 0; i < 10; i++){
                data.consume();
            }
        }, "B").start();
        new Thread(() ->{
            for(int i = 0; i < 10; i++){
                data.produce();
            }
        }, "C").start();
        new Thread(() ->{
            for(int i = 0; i < 10; i++){
                data.consume();
            }
        }, "D").start();
    }
}
class Data2{
    Lock lock = new ReentrantLock();
    Condition condition = lock.newCondition();
    int num = 0;
    // 步骤：判断等待、业务、通知唤醒
    public void produce(){
        lock.lock();
        try {
            while(num != 0){
                condition.await();
            }
            num++;
            System.out.println(Thread.currentThread().getName() + " -> " + num);
            condition.signalAll();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
    // 步骤：判断等待、业务、通知唤醒
    public void consume(){
        lock.lock();
        try {
            while(num == 0){
                condition.await();
            }
            num--;
            System.out.println(Thread.currentThread().getName() + " -> " + num);
            condition.signalAll();
        } catch(Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}
```
* 上面两种虽然可以实现生产者消费者问题，但是线程执行的顺序是不固定的，如果我们要想某个线程执行完之后指定另一个线程执行，则需要下面的方法。
### 1.4 使用 Condition 来精准地唤醒某个线程
```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

// Condition 可以用来唤醒指定的线程
// 现在有三个线程 A、B、C，现在要 A 线程执行完了之后去通知 B 线程执行，B 线程执行完了之后去通知 C 线程执行，C 线程执行完了之后去通知 A 线程执行
public class Test9 {
    public static void main(String[] args) {
        Data3 data = new Data3();
        new Thread(() ->{
            for(int i = 0; i < 10; i++){
                data.printA();
            }
        }, "A").start();
        new Thread(() ->{
            for(int i = 0; i < 10; i++){
                data.printB();
            }
        }, "B").start();
        new Thread(() ->{
            for(int i = 0; i < 10; i++){
                data.printC();
            }
        }, "C").start();
    }
}
class Data3{
    Lock lock = new ReentrantLock();
    // 通过监视器判断我要唤醒的是哪一个线程
    Condition condition1 = lock.newCondition();
    Condition condition2 = lock.newCondition();
    Condition condition3 = lock.newCondition();
    int num = 1; // num 为 1 时让线程 A 执行，num 为 2 时让线程 B 执行，num 为 3 时让线程 C 执行
    // 步骤：判断等待、业务、通知唤醒
    public void printA(){
        lock.lock();
        try {
            while(num != 1){
                condition1.await();
            }
            System.out.println(Thread.currentThread().getName() + " 线程执行了");
            num = 2;
            condition2.signal(); // 唤醒线程 B
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
    // 步骤：判断等待、业务、通知唤醒
    public void printB(){
        lock.lock();
        try {
            while(num != 2){
                condition2.await();
            }
            System.out.println(Thread.currentThread().getName() + " 线程执行了");
            num = 3;
            condition3.signal(); // 唤醒线程 C
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
    // 步骤：判断等待、业务、通知唤醒
    public void printC(){
        lock.lock();
        try {
            while(num != 3){
                condition3.await();
            }
            System.out.println(Thread.currentThread().getName() + " 线程执行了");
            num = 1;
            condition1.signal(); // 唤醒线程 A
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}
```
### 补充 1：synchronized 的使用方法
1. 修饰代码块。指定一个线程共享的对象，给其加锁
```java
synchronized(线程共享对象){
    同步代码块
}
```
2. 修饰实例方法，在实例方法上使用 synchronized。表示对当前实例对象 this 加锁，并且同步代码块是整个方法体。
```java
public synchronized void f(){}
```
3. 修饰静态方法，在静态方法上使用 synchronized。表示对当前类的 Class 对象加锁。多个线程执行同一个类中被 synchronized 修饰的静态方法时有且只有一个线程能够获取当前类的 Class 对象锁并执行方法，其它线程等待（注：一个类的类锁永远只有一把）
```java
public synchronized static void f(){}
```
### 补充 2：Lock 的使用方法
```java
Lock lock = new RenntrantLock();

lock.lock(); // 加锁
try{
    同步代码块
} catch (Exception e) {
    e.printStackTrace();
} finally {
    lock.unlock(); // 解锁
}
```
* Lock lock = new RenntrantLock(); 默认是非公平锁，传参 true 可变为公平锁。
### 补充 3：synchronized 和 Lock 的区别？
（1）synchronized 是内置的 Java 关键字，而 Lock 是一个 Java 接口
（2）synchronized 会自动释放锁，而 Lock 必须要手动释放锁
（3）synchronized：假设线程 A 获得锁，那么线程 B 只能阻塞等待，如果线程 A 在获得锁之后阻塞了，那么线程 B 只能一直等待；Lock：Lock 锁不一定会一直等待下去（因为有 tryLock() 方法）
（4）synchronized 是可重入锁，不可以中断，非公平锁；Lock 是可重入锁，可以判断锁，非公平/公平（可以自己设置）
（5）synchronized 适合锁住少量的同步代码，而 Lock 适合锁住大量的同步代码
## 二、集合类不安全
### 2.1 CopyOnWrite 思想
* 写入时复制（CopyOnWrite，简称COW）思想是计算机程序设计领域中的一种通用优化策略。其核心思想是，如果有多个调用者（Callers）同时访问相同的资源（如内存或者是磁盘上的数据存储），他们会共同获取相同的指针指向相同的资源，直到某个调用者修改资源内容时，系统才会真正复制一份专用副本（private copy）给该调用者，而其他调用者所见到的最初的资源仍然保持不变。这过程对其他的调用者都是透明的（transparently）。此做法主要的优点是如果调用者没有修改资源，就不会有副本（private copy）被创建，因此多个调用者只是读取操作的话可以共享同一份资源。
* 通俗易懂的讲，写入时复制技术就是不同进程在访问同一资源的时候，只有更新操作，才会去复制一份新的数据并更新替换，否则都是访问同一个资源。
* JDK 的 CopyOnWriteArrayList/CopyOnWriteArraySet 容器正是采用了 COW 思想。它是如何工作的呢？简单来说，就是**平时查询的时候都不需要加锁，随便访问。只有在更新的时候，才会从原来的数据复制一个副本出来，然后修改这个副本，最后把原数据替换成当前的副本。修改操作的同时，读操作不会被阻塞，而是继续读取旧的数据。**
### 2.2 List
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.Vector;
import java.util.concurrent.CopyOnWriteArrayList;

public class ListTest {
    // 在并发的情况下，ArrayList 是不安全的，会有：java.util.ConcurrentModificationException 并发修改异常
    // 如何变得安全：
    //     方案 1：List<String> list = new Vector<>();
    //     方案 2：List<String> list = Collections.synchronizedList(new ArrayList<>());
    //     方案 3：List<String> list = new CopyOnWriteArrayList<>();
    public static void main(String[] args) {
        // List<String> list = new ArrayList<>(); 在并发情况下不安全
        // List<String> list = new Vector<>();
        // List<String> list = Collections.synchronizedList(new ArrayList<>());

        // CopyOnWrite 写入时复制，简称 COW，它是计算机程序设计领域的一种优化策略。平时查询的时候都不需要加锁，随便访问。只有在更新的时候，
        // 才会从原来的数据复制一个副本出来，然后修改这个副本，最后把原数据替换成当前的副本。修改操作的同时，读操作不会被阻塞，而是继续读取旧的数据
        List<String> list = new CopyOnWriteArrayList<>();

        for(int i = 1; i <= 100; i++){
            new Thread(() -> {
                list.add("" + Math.random());
                System.out.println(list);
            }).start();
        }
    }
}
```
### 2.3 Set
```java
import java.util.Collections;
import java.util.HashSet;
import java.util.Set;
import java.util.concurrent.CopyOnWriteArraySet;

public class SetTest {
    // 在并发的情况下，HashSet 是不安全的，会有：java.util.ConcurrentModificationException 并发修改异常
    // 如何变得安全：
    //     方案 1：Set<String> set = Collections.synchronizedSet(new HashSet<>());
    //     方案 2：Set<String> set = new CopyOnWriteArraySet<>();
    public static void main(String[] args) {
        // Set<String> set = new HashSet<>(); 在并发情况下不安全
        // Set<String> set = Collections.synchronizedSet(new HashSet<>());

        // CopyOnWrite 写入时复制，简称 COW，它是计算机程序设计领域的一种优化策略。平时查询的时候都不需要加锁，随便访问。只有在更新的时候，
        // 才会从原来的数据复制一个副本出来，然后修改这个副本，最后把原数据替换成当前的副本。修改操作的同时，读操作不会被阻塞，而是继续读取旧的数据
        Set<String> set = new CopyOnWriteArraySet<>();
        for(int i = 1; i <= 100; i++){
            new Thread(() -> {
                set.add("" + Math.random());
                System.out.println(set);
            }).start();
        }
    }
    /*
      HashSet 的底层是什么？
         public HashSet() {
             map = new HashMap<>();
         }
         public boolean add(E e) {
             return map.put(e, PRESENT)==null; // PRESENT 是一个不变的值
         }
         private static final Object PRESENT = new Object();
     */
}
```
### 2.4 Map
```java
import java.util.Collections;
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.ConcurrentHashMap;

public class MapTest {
    // 在并发的情况下，HashMap 是不安全的，会有：java.util.ConcurrentModificationException 并发修改异常
    // 如何变得安全：
    //     方案 1：Map<String, String> map = Collections.synchronizedMap(new HashMap<>());
    //     方案 2：Map<String, String> map = new ConcurrentHashMap<>();
    public static void main(String[] args) {
        // Map<String, String> map = new HashMap<>();  在并发情况下不安全
        // Map<String, String> map = Collections.synchronizedMap(new HashMap<>());

        Map<String, String> map = new ConcurrentHashMap<>();
        for(int i = 1; i <= 100; i++){
            new Thread(() -> {
                map.put("" + Math.random(), "1");
                System.out.println(map);
            }).start();
        }
    }
}
```
## 三、Callable
* Callable 接口类似于 Runnable 接口，但又有所不同：
（1）Callable 接口是 call() 方法，Runnable 接口是 run() 方法。
（2）call() 方法可以有返回值，而 run() 方法没有返回值。
（3）call() 方法可以抛出异常，而 run() 方法不能抛出异常。
```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

public class CallableTest {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // new Thread(new Runnable).start()
        // new Thread(new FutureTask<V>()).start()   注：FutureTask 类实现了 Runnable 接口
        // new Thread(new FutureTask<V>( Callable )).start()

        MyThread3 thread = new MyThread3();
        FutureTask futureTask = new FutureTask(thread); // 适配类

        new Thread(futureTask).start();
        // new Thread(futureTask).start(); // 连续调用两个 start()，只会输出一个 "call 执行"

        // 注：这个 get() 方法可能会产生阻塞。因为 get() 方法需要一直等待 call() 方法的结果返回，如果 call() 方法需要执行很长时间，
        // 那么 get() 方法会一直等待直到 call() 方法执行完并返回结果后才能接着往下执行。因此一般把 get() 方法放到最后执行，或者采用异步通信的方式来处理
        String res = (String) futureTask.get(); // 获取 call() 方法的返回值
        System.out.println(res);
    }
}

class MyThread3 implements Callable<String>{
    @Override
    public String call() throws Exception {
        System.out.println("call 执行");
        return "jack";
    }
}
```
## 四、常用的辅助类
### 4.1 CountDownLatch 类
* CountDownLatch 能够使一个线程在等待另外一些线程完成一些工作之后再继续执行。使用一个计数器来实现（减法计数器）。当一个线程完成自己的任务之后，计数器的值就会减 1，当计数器的值减为 0 之后，表示其它线程已经完成了任务，然后在 CountDownLatch 上等待的线程就可以恢复执行接下来的任务。
```java
import java.util.concurrent.CountDownLatch;

public class CountDownLatchTest {
    // CountDownLatch 能够使一个线程在等待另外一些线程完成一些工作之后再继续执行。使用一个计数器来实现（减法计数器）。当一个线程完成自己的任务之后，
    // 计数器的值就会减 1，当计数器的值减为 0 之后，表示其它线程已经完成了任务，然后在 CountDownLatch 上等待的线程就可以恢复执行接下来的任务。
    // 常用方法：
    //     （1）countDown()：将计数器的值减 1
    //     （2）await()：当计数器的值归 0 时，才能继续向下执行
    // 每当有线程调用 countDown() 方法，会使计数器的值减 1，当计数器的值减为 0 时，await() 方法就会被唤醒，然后继续往下执行
    public static void main(String[] args) throws InterruptedException {
        // 比如：一个班级里有 6 个人，当这 6 个人都走了之后才能把班级的门关上。

        CountDownLatch countDownLatch = new CountDownLatch(6); // 总数是 6
        for(int i = 1; i <= 6; i++){
            new Thread(() -> {
                System.out.println("学生" + Thread.currentThread().getName() + " 出去了");
                countDownLatch.countDown(); // 计数器的值 - 1
            }, String.valueOf(i)).start();
        }
        countDownLatch.await(); // 等待计数器归 0，才能继续向下执行
        System.out.println("关门");

    }
}
```
### 4.2 CyclicBarrier 类
* CycleBarrier 能够使一组线程相互等待，当所有线程都到达各自的屏障点后再进行后续的操作。使用一个计数器来实现（加法计数器）。
```java
import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

public class CyclicBarrierTest {
    // CycleBarrier 能够使一组线程相互等待，当所有线程都到达各自的屏障点后再进行后续的操作。使用一个计数器来实现（加法计数器）。
    public static void main(String[] args) {
        // 比如：集齐 7 颗龙珠后召唤神龙
        CyclicBarrier cyclicBarrier = new CyclicBarrier(7, new Thread(() -> {
            System.out.println("召唤神龙！");
        })); // 注：CyclicBarrier 类是通过 await() 方法来计数的

        for(int i = 1; i <= 7; i++){
            final int temp = i; // 必须要有 final，在 jdk8 以后，可以不写 final ，但是会默认加上 final。在 lambda 里面是修改不了 temp 的值的
            new Thread(() -> {
                System.out.println("拿到了第 " + temp + " 颗龙珠"); // 在 lambda 里面是拿不到 i 的
                try {
                    cyclicBarrier.await(); // 线程调用 await() 方法表示自己已经到达栅栏。执行完 await() 方法之后当前线程就会被阻塞
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } catch (BrokenBarrierException e) {
                    e.printStackTrace();
                }
                System.out.println("->" + temp); // 输出了 召唤神龙！ 之后，所有的线程才会执行这行代码
            }).start();
        }
    }
}
```
### 4.3 Semaphore 类
```java
import java.util.concurrent.Semaphore;

public class SemaphoreTest {
    // Semaphore 类的作用：（1）多个共享资源的互斥使用 （2）并发限流，用于限制可以访问某些资源的最大线程数
    public static void main(String[] args) {
        // 比如：一共有 3 个车位，6 辆车
        Semaphore semaphore = new Semaphore(3);
        for(int i = 1; i <= 6; i++){
            new Thread(() -> {
                try {
                    semaphore.acquire(); // 获得一个信号量，如果没有则当前线程等待
                    System.out.println(Thread.currentThread().getName() + " 获得了车位");
                    Thread.sleep(2000); // 占据车位 2 秒钟
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    semaphore.release(); // 释放一个信号量
                    System.out.println(Thread.currentThread().getName() + " 释放了车位");
                }
            }, String.valueOf(i)).start();
        }
    }
}
```
## 五、读写锁
```java
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class ReadWriteLockTest {
    // 独占锁（写锁）：一次只能被一个线程占有
    // 共享锁（读锁）：一次可以被多个线程同时占有
    // ReadWriteLock 维护一对关联的锁，一个用于只读操作，一个用于写入。读锁可以在没有写锁的时候被多个线程同时持有，写锁是独占的
    public static void main(String[] args) {
        // MyCache myCache = new MyCache();
        MyCache1 myCache = new MyCache1();
        // 存入
        for(int i = 1; i <= 5; i++){
            final int temp = i;
            new Thread(() -> {
                myCache.put(temp + "", temp + "");
            }, "线程" + i).start();
        }
        // 读取
        for(int i = 1; i <= 5; i++){
            final int temp = i;
            new Thread(() -> {
                myCache.get(temp + "");
            }, "线程" + i).start();
        }
    }
}
// 自定义缓存
class MyCache{
    private volatile Map<String, Object> map = new HashMap<>();
    // 存入数据（写）
    public void put(String key, Object value){
        System.out.println(Thread.currentThread().getName() + " 写入：" + key);
        map.put(key, value);
        System.out.println(Thread.currentThread().getName() + " 写入成功");
    }
    // 取出数据（读）
    public Object get(String key){
        System.out.println(Thread.currentThread().getName() + " 读取：" + key);
        Object o = map.get(key);
        System.out.println(Thread.currentThread().getName() + " 读取成功");
        return o;
    }
}
// 加锁的自定义缓存
class MyCache1{
    private volatile Map<String, Object> map = new HashMap<>();
    // 读写锁，更加细粒度的锁
    private ReadWriteLock readWriteLock = new ReentrantReadWriteLock();
    // 存入数据（写）
    public void put(String key, Object value){
        readWriteLock.writeLock().lock();
        try{
            System.out.println(Thread.currentThread().getName() + " 写入：" + key);
            map.put(key, value);
            System.out.println(Thread.currentThread().getName() + " 写入成功");
        } catch (Exception e){
            e.printStackTrace();
        } finally{
            readWriteLock.writeLock().unlock();
        }
    }
    // 取出数据（读）
    public Object get(String key){
        readWriteLock.readLock().lock();
        try{
            System.out.println(Thread.currentThread().getName() + " 读取：" + key);
            Object o = map.get(key);
            System.out.println(Thread.currentThread().getName() + " 读取成功");
            return o;
        }catch(Exception e){
            e.printStackTrace();
        } finally{
            readWriteLock.readLock().unlock();
        }
        return null;
    }
}
```
## 六、阻塞队列
* 阻塞：
（1）在写入数据时，如果队列满了，就必须阻塞等待。
（2）再读出数据时，如果队列为空，也必须阻塞等待。
### 6.1 BlockingQueue 的关系图
![图示](https://img-blog.csdnimg.cn/b2b2ced454684dd2a73168a32cf7ee10.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
### 6.1 BlockingQueue 的四组 API
1. 添加数据
（1）boolean add(E e)：添加一个元素 e 到队列中，如果队列已满，则抛出异常。
（2）boolean offer(E e)：添加一个元素 e 到队列中，如果队列已满，则返回 false。　　　　　 
（3）void put(E e)：添加一个元素 e 到队列中，如果队列已满，那么再调用该方法添加元素时会一直阻塞等待。
（4）boolean offer(E e, long timeout, TimeUnit unit)：添加一个元素 e 到队列中，如果队列已满，那么再调用该方法添加元素时会阻塞相应的时间（timeout 指定超时时间，unit 指定超时时间的单位），如果在该时间内队列还是满的，即无法将元素添加到队列里，则返回 false。
2. 获取数据
（1）E remove()：移除队首元素并返回，如果队列为空，则抛出异常。
（2）E poll()：移除队首元素并返回，如果队列为空，则返回 null。
（3）E take()：移除队首元素并返回，如果队列为空，那么再调用该方法移除元素时会一直阻塞等待。
（4）E poll(long timeout, TimeUnit unit)：移除队首元素并返回，如果队列为空，那么再调用该方法移除元素时会阻塞相应的时间（timeout 指定超时时间，unit 指定超时时间的单位），如果在该时间内队列还是空的，即无法移除队列中的元素，则返回 null。
3. 查看队首元素
（1）E element()：查看队首元素并返回，如果队列为空，则抛出异常。
（2）E peek()：查看队首元素并返回，如果队列为空，则返回 null。

|              | 抛出异常  | 有返回值，不会抛出异常 | 一直阻塞等待 | 超时阻塞等待                            |
| ------------ | --------- | ---------------------- | ------------ | --------------------------------------- |
| 添加元素     | add(E e)  | offer(E e)             | put(E e)     | offer(E e, long timeout, TimeUnit unit) |
| 移除元素     | remove()  | poll()                 | take()       | poll(long timeout, TimeUnit unit)       |
| 查看队首元素 | element() | peek()                 | \            | \                                       |
```java
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.TimeUnit;

public class BlockingQueueTest {
    public static void main(String[] args) throws InterruptedException {
        test2();
    }
    // 抛出异常
    public static void test1(){
        // 队列大小为 3
        BlockingQueue blockingQueue = new ArrayBlockingQueue(3);
        System.out.println(blockingQueue.add("a"));
        System.out.println(blockingQueue.add("b"));
        System.out.println(blockingQueue.add("c"));
        // 如果队列已经满了，再调用 add() 方法添加元素时，会抛出异常：java.lang.IllegalStateException: Queue full
        // System.out.println(blockingQueue.add("d"));

        System.out.println(blockingQueue.remove());
        System.out.println(blockingQueue.remove());
        System.out.println(blockingQueue.remove());
        // 如果队列为空，再调用 remove() 方法移除元素时，会抛出异常：java.util.NoSuchElementException
        // System.out.println(blockingQueue.remove());

        // 如果队列为空，再调用 element() 方法查看队首元素时，会抛出异常：java.util.NoSuchElementException
        System.out.println(blockingQueue.element());
    }
    // 有返回值，不会抛出异常
    public static void test2(){
        // 队列大小为 3
        BlockingQueue blockingQueue = new ArrayBlockingQueue(3);
        System.out.println(blockingQueue.offer("a"));
        System.out.println(blockingQueue.offer("b"));
        System.out.println(blockingQueue.offer("c"));
        // 如果队列已经满了，再调用 offer() 方法添加元素时，会返回 false
        System.out.println(blockingQueue.offer("d"));

        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        // 如果队列为空，再调用 poll() 方法移除元素时，会返回 null
        System.out.println(blockingQueue.poll());

        // 如果队列为空，再调用 peek() 方法查看队首元素时，会返回 null
        System.out.println(blockingQueue.peek());
    }

    // 如果队列满了再加元素或者队列为空再取元素时，会阻塞等待（一直阻塞等待）
    public static void test3() throws InterruptedException {
        // 队列大小为 3
        BlockingQueue blockingQueue = new ArrayBlockingQueue(3);
        blockingQueue.put("a");
        blockingQueue.put("b");
        blockingQueue.put("c");
        // 如果队列已经满了，再调用 put() 方法添加元素时，会一直阻塞等待
        // blockingQueue.put("d");

        System.out.println(blockingQueue.take());
        System.out.println(blockingQueue.take());
        System.out.println(blockingQueue.take());
        // 如果队列为空，再调用 take() 方法移除元素时，会一直阻塞等待
        System.out.println(blockingQueue.take());
    }

    // 如果队列满了再加元素或者队列为空再取元素时，会阻塞等待（超时等待）
    public static void test4() throws InterruptedException {
        // 队列大小为 3
        BlockingQueue blockingQueue = new ArrayBlockingQueue(3);
        System.out.println(blockingQueue.offer("a"));
        System.out.println(blockingQueue.offer("b"));
        System.out.println(blockingQueue.offer("c"));
        // 如果队列已经满了，再调用 offer() 方法添加元素时，会阻塞等待 2 秒，如果在这 2 秒内队列还是满的，则会超时退出，并返回 false
        System.out.println(blockingQueue.offer("d", 2, TimeUnit.SECONDS)); // 参数分别为：要加入队列的元素，超时时间和超时时间的单位。

        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        // 如果队列为空，再调用 poll() 方法移除元素时，会阻塞等待 3 秒，如果在这 3 秒内队列还是空的，则会超时退出，并返回 null
        System.out.println(blockingQueue.poll(3, TimeUnit.SECONDS));
    }
}
```
### 6.2 SynchronousQueue 同步队列
* 注：SynchronousQueue 没有容量。SynchronousQueue是一个内部只能包含一个元素的队列。**某个线程将一个元素加入到同步队列后，该线程就会被直接阻塞，只有当另一个线程从同步队列中取出了队列中存储的元素，该线程才能结束阻塞。示例：**
```java
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.SynchronousQueue;

public class SynchronousQueueTest {
    public static void main(String[] args) {
        // 以下代码是输出不了 123 的。因为在 put 进一个元素 1 之后就会直接阻塞当前线程，只有当 take() 出来元素后才能继续执行后续代码
        BlockingQueue blockingQueue = new SynchronousQueue();
        try {
            blockingQueue.put(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("123");
    }
}
public class SynchronousQueueTest1 {
    public static void main(String[] args) {
        // 以下代码在过了 3 秒后才能输出 123。线程A 在执行了 queue.put(1) 后就被阻塞了，只有当线程B 执行了 take() 方法之后，线程A 才可以继续执行。
        BlockingQueue blockingQueue = new SynchronousQueue();
        new Thread(() -> {
            try {
                blockingQueue.put(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("123");
        }, "线程A").start();

        new Thread(() -> {
            try {
                Thread.sleep(3000);
                blockingQueue.take();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }, "线程B").start();
    }
}
```
```java
public class SynchronousQueueTest {
    public static void main(String[] args) {
        BlockingQueue blockingQueue = new SynchronousQueue();

        new Thread(() -> {
            try {
                System.out.println(Thread.currentThread().getName() + " 放入了 1");
                blockingQueue.put("1");
                
                System.out.println(Thread.currentThread().getName() + " 放入了 2");
                blockingQueue.put("2");
                
                System.out.println(Thread.currentThread().getName() + " 放入了 3");
                blockingQueue.put("3");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }, "线程 A").start();

        new Thread(() -> {
            try {
                Thread.sleep(3000);
                System.out.println(Thread.currentThread().getName() + " 取出了 1");
                blockingQueue.take();
                
                Thread.sleep(3000);
                System.out.println(Thread.currentThread().getName() + " 取出了 2");
                blockingQueue.take();
                
                Thread.sleep(3000);
                System.out.println(Thread.currentThread().getName() + " 取出了 3");
                blockingQueue.take();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }, "线程 B").start();
    }
}
```
## 七、线程池
* **频繁地创建和销毁线程会消耗大量的系统资源，效率很低**。因此我们可以使用线程池来管理线程资源：**在任务执行前，从线程池中拿出线程来执行；在任务执行完成之后，再把线程放回线程池。通过线程池，我们可以有效地避免频繁创建和销毁线程。**
* 线程池的作用：
（1）**降低系统资源消耗**。线程本身是一种资源，创建和销毁线程会有开销，通过线程池去**复用已存在的线程（线程复用）** 来降低资源消耗。复用：向线程池拿一个线程，当工作完成后，并不是直接关闭线程，而是将这个线程归还给线程池供其他任务使用。
（2）**提高系统响应速度**。当有任务到达时，无需等待新线程的创建，直接从线程池中获取线程便能立即执行。
（3）**方便对线程并发数的管理**。线程不能无限制地创建，需要进行统一的分配、调优和监控。如果线程数过多，可能会导致内存占用过多而产生 OOM，并且会造成cpu过度切换，导致效率降低。
（4）线程池可以帮助我们维护线程 ID、线程状态等信息，也可以帮助我们维护任务执行状态等信息。
### 7.1 线程池三大方法
>1、 ExecutorService threadPool = Executors.newSingleThreadExecutor(); // 创建只有一个线程的线程池
>2、ExecutorService threadPool = Executors.newFixedThreadPool(5); // 创建一个固定大小的线程池
>3、ExecutorService threadPool = Executors.newCachedThreadPool(); // 创建一个可缓存的线程池，如果没有可用的线程，则创建一个新线程并添加到池中
```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class PoolTest01 {
    public static void main(String[] args) {
        // Executors 工具类，类似于 Collections、Arrays。Executors 里面有三大方法：
        // ExecutorService threadPool = Executors.newSingleThreadExecutor(); // 创建只有一个线程的线程池
        // ExecutorService threadPool = Executors.newFixedThreadPool(5); // 创建一个固定大小的线程池
        ExecutorService threadPool = Executors.newCachedThreadPool(); // 创建一个可缓存的线程池，如果没有可用的线程，则创建一个新线程并添加到池中

        try {
            for (int i = 0; i < 100; i++) {
                // 使用线程池来创建线程
                threadPool.execute(() -> {
                    System.out.println(Thread.currentThread().getName() + " ok");
                });
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // 关闭线程池
            threadPool.shutdown();
        }
    }
}
```
* **注：在阿里开发手册中，不允许我们使用上述三种方式去创建线程池，而应该直接使用 ThreadPoolExecutor 的方式去创建线程池。**
![图示](https://img-blog.csdnimg.cn/5a10783a53924bd6a13431412145d211.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
### 7.2 线程池七大参数
* 上面的三个方法本质上也是创建的 **ThreadPoolExecutor** 对象。
```java
public static ExecutorService newSingleThreadExecutor() {
    return new FinalizableDelegatedExecutorService
            (new ThreadPoolExecutor(1, 1,
                                0L, TimeUnit.MILLISECONDS,
                                new LinkedBlockingQueue<Runnable>()));
}
public static ExecutorService newFixedThreadPool(int nThreads) {
    return new ThreadPoolExecutor(nThreads, nThreads,
                                  0L, TimeUnit.MILLISECONDS,
                                  new LinkedBlockingQueue<Runnable>());
}
public static ExecutorService newCachedThreadPool() {
    return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                  60L, TimeUnit.SECONDS,
                                  new SynchronousQueue<Runnable>());
}
```
* ThreadPoolExecutor 类的七大参数：
```java
public ThreadPoolExecutor(int corePoolSize, // 核心线程数
                          int maximumPoolSize, // 线程池所允许的最大线程个数
                          long keepAliveTime, // 超过这个时间，多余的线程会被回收，直到线程池中的线程数小于等于核心线程数
                          TimeUnit unit, // 超时时间的单位
                          BlockingQueue<Runnable> workQueue, // 阻塞队列
                          ThreadFactory threadFactory, // 线程工厂，用于创建线程
                          RejectedExecutionHandler handler /*拒绝策略*/) {
    if (corePoolSize < 0 ||
        maximumPoolSize <= 0 ||
        maximumPoolSize < corePoolSize ||
        keepAliveTime < 0)
        throw new IllegalArgumentException();
    if (workQueue == null || threadFactory == null || handler == null)
        throw new NullPointerException();
    this.acc = System.getSecurityManager() == null ?
            null :
        AccessController.getContext();
    this.corePoolSize = corePoolSize;
    this.maximumPoolSize = maximumPoolSize;
    this.workQueue = workQueue;
    this.keepAliveTime = unit.toNanos(keepAliveTime);
    this.threadFactory = threadFactory;
    this.handler = handler;
}
```
1. **corePoolSize：核心线程数**，也是线程池长期维持的线程数。当提交一个任务到线程池时，线程池会创建一个线程来执行任务，即使其他空闲的核心线程能够执行新任务也会创建线程，等到需要执行的任务数大于核心线程数时就不再创建。
2. **maximumPoolSize：线程池所允许的最大线程个数**。在核心线程数的基础上可能会额外增加一些非核心线程，只有当阻塞队列 workQueue 填满时才会创建多于 corePoolSize 的线程（线程池总线程数不超过 maximumPoolSize）
3. **keepAliveTime：非核心线程的存活保持时间**。非核心线程的空闲时间如果超过了 keepAliveTime，那么这个非核心线程就会被自动终止回收掉。注：当 corePoolSize 等于 maximumPoolSize 时，keepAliveTime 参数也就不起作用了，因为不存在非核心线程。
4. **unit：keepAliveTime 的时间单位**
5. **workQueue：任务队列，用于保存任务的阻塞队列。**
6. **threadFactory：线程工厂，用于创建新的线程。**
7. **handler：拒绝策略**。当线程池和阻塞队列都满了，再加入的线程会执行此策略。四种拒绝策略：AbortPolicy、CallerRunsPolicy、DiscardOldestPolicy、DiscardPolicy
* 线程池中的线程创建流程：
![图示](https://img-blog.csdnimg.cn/4396ebcc64c843a2960d7b3c401f9156.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
### 7.3 线程池四大拒绝策略
1. **AbortPolicy**：当阻塞队列和线程池都满了，任务再添加到线程池时，**会抛出异常**
2. **DiscardPolicy**：当阻塞队列和线程池都满了，任务再添加到线程池时，**该任务会被直接丢弃，不会抛出异常**。
因此这种策略存在一定的风险，因为我们根本不知道提交任务有没有被丢弃，可能造成数据丢失。
3. **DiscardOldestPolicy**：阻塞队列和线程池都满了，任务再添加到线程池时，**会将队列中的第一个元素移除，再尝试添加，如果添加失败，则按该策略不断尝试。**
特点：这个策略还是会丢弃任务，丢弃时也是悄无声息，不过它丢弃的是队列中的头节点，也就是存活时间最久的
4. **CallerRunsPolicy**：当阻塞队列和线程池都满了，任务再添加到线程池时，**会把这个任务交于提交该任务的线程执行，也就是谁提交的任务，谁就负责执行该任务。**
这样做主要有两点好处：第一点是新提交的任务不会被丢弃，这样也就不会造成业务损失。 第二点是，由于谁提交任务谁就要负责执行任务，这样提交任务的线程就得负责执行任务，而执行任务又是比较耗时的，在这段期间，提交任务的线程被占用，也就不会再提交新的任务，减缓了任务提交的速度，相当于是一个负反馈。在此期间，线程池中的线程也可以充分利用这段时间来执行掉一部分任务，腾出一定的空间，相当于是给了线程池一定的缓冲期。
```java
import java.util.concurrent.*;

public class PoolTest02 {
    public static void main(String[] args) {
        // new ThreadPoolExecutor.AbortPolicy() 当阻塞队列和线程池都满了，任务再添加到线程池时，会抛出异常
        // new ThreadPoolExecutor.DiscardPolicy() 当阻塞队列和线程池都满了，任务再添加到线程池时，该任务会被直接丢弃，不会抛出异常
        // new ThreadPoolExecutor.DiscardOldestPolicy() 阻塞队列和线程池都满了，任务再添加到线程池时，会将队列中的第一个元素移除，再尝试添加，如果添加失败，则按该策略不断尝试
        // new ThreadPoolExecutor.CallerRunsPolicy() 当阻塞队列和线程池都满了，任务再添加到线程池时，会把这个任务交于提交该任务的线程执行，也就是谁提交的任务，谁就负责执行该任务。
        ExecutorService threadPool = new ThreadPoolExecutor(
                2,
                5,
                3,
                TimeUnit.SECONDS,
                new LinkedBlockingQueue<>(3),
                Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.DiscardOldestPolicy()
        );
        try {
            for (int i = 0; i < 9; i++) {
                // 使用线程池来创建线程（即将任务添加到线程池）
                threadPool.execute(() -> {
                    System.out.println(Thread.currentThread().getName() + " ok");
                });
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // 关闭线程池
            threadPool.shutdown();
        }
    }
}
```
### 7.4 CPU 密集型和 I/O 密集型
#### 线程池的最大大小 maximumPoolSize 怎么设计?
&nbsp;&nbsp;（1）CPU 密集型：CPU 是几核，那么 maximumPoolSize 就是几。即 maximumPoolSize = CPU 核数
&nbsp;&nbsp;（2）I/O 密集型：当一个线程执行 I/O 操作导致 CPU 空闲时（I/O 操作不会占用 CPU），处理器可以进行上下文切换来处理其他线程。如果我们只有 CPU 核心数那么多个线程的话，那么即使有待执行的任务，我们也没有更多的线程去处理这些任务。因此我们判断你的线程中十分耗 I/O 的线程，maximumPoolSize 只要大于这个数即可。一般 maximumPoolSize = 2 * CPU 核数
* 获取 CPU 的核数
```java
System.out.println(Runtime.getRuntime().availableProcessors());
```
## 八、四大函数式接口
* **函数式接口就是有且只有一个抽象方法的接口。**
* 只要是函数式接口，就可以使用 Lambda 表达式简化。
### 8.1 Function 函数型接口
```java
@FunctionalInterface
public interface Function<T, R> {
    // 传入参数 T，返回类型 R
    R apply(T t);
}
```
```java
import java.util.function.Function;

public class FunctionTest {
    public static void main(String[] args) {
        Function<Integer, String> function = new Function<Integer, String>() {
            @Override
            public String apply(Integer integer) {
                return integer + "";
            }
        }; // 等式右边就是一个匿名内部类
        System.out.println(function.apply(123));

        Function<Integer, String> function = (integer) -> { return integer + ""; };
        System.out.println(function.apply(123));

    }
}
```
### 8.2 Predicate 断定型接口
```java
@FunctionalInterface
public interface Predicate<T> {
    // 有一个输入参数，返回值是布尔类型的
    boolean test(T t);
}
```
```java
import java.util.function.Predicate;

public class PredicateTest {
    public static void main(String[] args) {
        Predicate<String> predicate = new Predicate<String>() {
            @Override
            public boolean test(String s) {
                return s.isEmpty();
            }
        }; // 等式右边就是一个匿名内部类
        System.out.println(predicate.test("abc"));

        Predicate<String> predicate = (s) -> { return s.isEmpty(); };
        System.out.println(predicate.test("abc"));
    }
}
```
### 8.3 Consumer 消费型接口
```java
@FunctionalInterface
public interface Consumer<T> {
    // 有一个参数，没有返回值
    void accept(T t);
}
```
```java
import java.util.function.Consumer;

public class ConsumerTest {
    public static void main(String[] args) {
        Consumer<String> consumer = new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        };// 等式右边就是一个匿名内部类
        consumer.accept("abcd");

        Consumer consumer = (s) -> { System.out.println(s); };
        consumer.accept("abcd");
    }
}
```
### 8.4 Supplier 供给型接口
```java
@FunctionalInterface
public interface Supplier<T> {
    // 没有参数，只有返回值
    T get();
}
```
```java
import java.util.function.Supplier;

public class SupplierTest {
    public static void main(String[] args) {
        Supplier<String> supplier = new Supplier<String>() {
            @Override
            public String get() {
                System.out.println("get 方法执行了");
                return "abcd";
            }
        }; // 等式右边就是一个匿名内部类
        System.out.println(supplier.get());

        Supplier<String> supplier = () -> {
            System.out.println("get 方法执行了");
            return "abcd";
        };
        System.out.println(supplier.get());
    }
}
```
## 九、Stream 流式计算

```java
import java.util.Arrays;
import java.util.List;

public class StreamTest {
    /**
     * 现有5个用户，按以下要求筛选：
     * 1. id 是偶数的
     * 2. 年纪大于 23 岁
     * 3. 用户名转为大写字母
     * 4. 按照用户名倒序排序
     * 5. 只输出一个用户
     */
    public static void main(String[] args) {
        User u1 = new User(1, "a", 21);
        User u2 = new User(2, "b", 22);
        User u3 = new User(3, "c", 23);
        User u4 = new User(4, "d", 24);
        User u5 = new User(6, "e", 25);
        // 集合就是用来存储数据
        List<User> list = Arrays.asList(u1, u2, u3, u4, u5);
        // 计算交给 Stream 流
        // // 下面的代码包含了：Lambda 表达式、链式编程、函数式接口、Stream 流式计算
        list.stream()
                .filter((u) -> { return u.getId() % 2 == 0; })
                .filter((u) -> (u.getAge() > 23))
                .map((u) -> { return u.getName().toUpperCase(); })
                .sorted((uu1, uu2) -> { return uu2.compareTo(uu1); }) // 因为前面的 map 返回的是一个字符串，所以这里操作的就是字符串了
                .limit(1)
                .forEach(System.out::println);
    }
}
class User {
    private int id;
    private String name;
    private int age;
    public User(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
    public String toString() { return "User{" +"id=" + id +", name='" + name + '\'' +", age=" + age +'}'; }
}
```
## 十、异步回调
* 同步和异步，描述的是调用者，要不要主动等待函数的返回值
（1）同步：调用了一个方法后，调用者必须等待被调用的方法执行完成后，才能接着执行后续的代码。
（2）异步：调用了一个方法后，这个调用就直接返回了，调用者无需等待被调用的方法执行完成，就可以继续执行后续的代码。然后可以利用回调或者事件通知的方式得知被调用方法的结果。
* 阻塞和非阻塞，描述的是函数本身，在等待某一事件的结果时，是将线程挂起，还是立即返回一个未就绪等信息
（1）阻塞：调用了一个方法后，在调用的结果返回之前，当前线程会被阻塞挂起
（2）非阻塞：调用了一个方法后，在调用结果返回之前，这个调用不会阻塞当前线程
* 所以同步和阻塞看起来都是等，但是本质上它们不一样，同步的时候不会让出 CPU
```java
import java.util.concurrent.CompletableFuture;

public class YiBuTest {
    public static void main(String[] args) throws Exception {
        // 没有返回值的异步任务
        CompletableFuture<Void> completableFuture = CompletableFuture.runAsync(() -> {
            System.out.println(Thread.currentThread().getName() + "正在执行一个没有返回值的异步任务");
            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        completableFuture.get();
        System.out.println("1111");

        // 有返回值的异步任务
        CompletableFuture<Integer> completableFuture = CompletableFuture.supplyAsync(() -> {
            System.out.println(Thread.currentThread().getName()+"正在执行一个有返回值的异步任务。");
            int num = 5/0;
            return 1024;
        });
        completableFuture.whenComplete((t, u) -> {
            System.out.println("t = " + t); // 方法的返回值
            System.out.println("u = " + u); // 方法过程中异常的信息
        }).get();
    }
}
```
## 十一、Volatile
* Volatile 是 Java 虚拟机提供的轻量级的同步机制。
* Volatile 的三个特性：
（1）保证可见性
（2）不保证原子性
（3）禁止指令重排（保证有序性）
* 可见性：当一个线程修改了线程共享变量的值，其它线程能够立即得知这个修改。
* 原子性：一个操作，要么全都执行，要么全都不执行。
* 有序性：程序执行的顺序按照代码的先后顺序执行。
### 11.1 Java 内存模型（JMM）
* JMM 规定了**所有的变量都存储在主内存中。每个线程还有自己的工作内存，线程的工作内存中保存了该线程使用到的变量的主内存的副本拷贝，线程对变量的所有操作（读取、赋值等）都必须在工作内存中进行，而不能直接读写主内存中的变量**（volatile 变量仍然有工作内存的拷贝，但是由于它特殊的操作顺序性规定，所以看起来如同直接在主内存中读写访问一般）。不同的线程之间也无法直接访问对方工作内存中的变量，线程之间值的传递都需要通过主内存来完成。
* **当线程读取变量前，必须读取主存中的最新值到工作内存中。
当线程更改完变量之后，必须立刻把新的值刷新回主存。**
* 比如主存中有一个变量：flag = true，线程 A 想要使用该变量就会把该变量拷贝到自己的工作内存中，后续 A 线程操作的就是拷贝在工作线程中的这个变量。此时可能会出现问题，比如另一个线程 B 也把这个变量拷贝到了自己的工作内存中，并且将值改为了 false，然后将该值刷回到了主存中，但是此时线程 A 操作的还是自己工作线程中的那个 flag = true，它并不知道这个值已经发生了修改，也就是说此时是不可见的。
```java
public class VolatileTest01 {
    // 不加 volatile 程序会一直陷入死循环
    // 加了 volatile 后可以保证可见性
    public volatile static boolean flag = true;
    public static void main(String[] args) {
        new Thread(() -> {
            while(flag){

            }
        }).start();
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        flag = false;
        System.out.println(flag);
    }
}
```
### 11.2 不保证原子性
```java
public class VolatileTest02 {
    // 不加 volatile，最后结果不一定是 20000
    // 即使加了 volatile，最后结果也不一定是 20000。即 volatile 不保证原子性
    private volatile static int num = 0;
    public static void main(String[] args) {
        for(int i = 1; i <= 20; i++){
            new Thread(() -> {
                for(int j = 1; j <= 1000; j++){
                    add();
                }
            }).start();
        }
        // 让上面的 20 条线程都执行完。如果存活线程的数量 > 2，就说明上面还有线程没执行完。因为 Java 里面有两个线程是默认执行的：main、gc
        while(Thread.activeCount() > 2){
            Thread.yield();
        }
        System.out.println(num);
    }
    public static void add(){
        num++; // num++ 不是原子性操作。大体有三个步骤：获得 num 的值；加 1；写回 num 的值
    }
}
```
* 如果不使用 Lock 和 Synchronized，还有其它方法可以保证原子性吗？
可以使用原子类。原子类非常高效
```java
import java.util.concurrent.atomic.AtomicInteger;

public class VolatileTest03 {
    // 使用原子类可以保证原子性
    private volatile static AtomicInteger num = new AtomicInteger(0);
    public static void main(String[] args) {
        for(int i = 1; i <= 20; i++){
            new Thread(() -> {
                for(int j = 1; j <= 1000; j++){
                    add();
                }
            }).start();
        }
        // 让上面的 20 条线程都执行完。如果存活线程的数量 > 2，就说明上面还有线程没执行完。因为 Java 里面有两个线程是默认执行的：main、gc
        while(Thread.activeCount() > 2){
            Thread.yield();
        }
        System.out.println(num);
    }
    public static void add(){
        num.getAndIncrement(); // + 1 的方法。底层：CAS
    }
}
```
### 11.3 禁止指令重排
* 什么是指令重排？
程序在执行时，为了提高性能，编译器和处理器常常会对指令做重排序。比如：
```java
int x = 0;
int y = 1;
```
按照我们常规的想法，这段代码应该从上往下依次执行，但是真实情况是，JVM 会在真正执行这段代码的时候进行优化，发生指令的重排序，因此有可能是先执行 y = 1 ，再执行 x = 0
* 处理器在进行指令重排的时候，是会考虑到指令之间的数据依赖性的。比如：

```java
int x = 1; // 语句 1
int y = 2; // 语句 2
int z = x + y; // 语句 3
```
我们期望的是 1、2、3 顺序执行，但实际可能是按 2、1、3，但是不可能会按照 3、1、2 来执行。因为上述代码中 x 和 y 不存在依赖关系，所以 1、2 可以进行重排序；z 依赖 x 和 y，所以 3 必须在 1、2 的后面执行。处理器在进行重排序时是会考虑到指令之间的数据依赖性，如果一个指令必须用到另一个指令的结果，那么处理器一定会保证另一个指令先执行。
* 指令重排在单线程环境下一点问题都没有，但是在多线程环境下就可能出现错误。
```java
// 下面的代码由于语句 1 和语句 2 之间没有数据依赖性，因此可能会发生指令重排。
// 如果一个线程执行 init() 方法，一个线程执行 use() 方法，那么下面代码的执行顺序可能是：
//   （1）语句 1 先执行，语句 2 后执行：语句 1 -> 语句 2 -> 语句 3 -> 语句 4。此时结果：i = 1
//   （2）语句 2 先执行，语句 1 后执行：语句 2 -> 语句 3 -> 语句 4 -> 语句 1。此时结果：i = 0
int a = 0;
boolean flag = false;
public void init(){
    a = 1; // 语句 1
    flag = true; // 语句 2
}
public void use(){
    if(flag){ // 语句 3
        int i = a * a; // 语句 4
    }
}
```
* volatile 是如何禁止指令重排的？ 
使用内存屏障可以禁止指令重排。内存屏障其实就是一个 CPU 指令，如果我们使用内存屏障来禁止指令重排，那么 Java 编译器在生成指令序列时，会插入特定类型的内存屏障指令，通过内存屏障指令来禁止重排序。内存屏障主要有两个作用：
（1）阻止屏障两侧的指令重排序，保证特定指令的执行顺序。
（2）强制把本地内存中的脏数据刷新回主内存，保证了可见性
## 十二、单例模式
### 12.1 基础知识
* 什么是单例模式？
 单例模式顾名思义，就是**指在整个运行时域，一个类只有一个实例对象**
* 为什么需要单例模式？
有的类的实例对象的创建和销毁对资源来说消耗不大，比如 String。但是有的类比较庞大和复杂，如果频繁地创建和销毁对象，并且这些对象完全是可以复用的话，那么将会造成一些不必要的性能浪费，比如：我现在要访问数据库，而创建数据库连接操作是一个耗资源的操作，并且数据库连接完全是可以复用的，那么我可以将这个对象设计成单例的，这样我只需要创建一次并且重复使用这个对象就行了，而不是每次访问数据库都去创建一个新的连接对象
### 12.2 饿汉式单例
> * 优点：写法比较简单；而且在类装载的时候就完成了对象的实例化，没有线程安全问题。
> 为什么没有线程安全问题？JVM 虚拟机必须保证一个类的类构造器 \<clinit>() 在多线程环境中被正确地加锁同步。如果多个线程同时去初始化一个类，那么只会有其中一个线程去执行这个类的 \<clinit>() 方法，其他线程都需要阻塞等待，直到活动线程执行 \<clinit>() 方法完毕，而且其他线程在被唤醒后不会再执行 \<clinit>() 方法。所以**在同一个类加载器下，一个类只会被初始化一次。**
> * 缺点：实例对象在程序启动的时候就已经创建好了，如果这个对象是比较大的，而且我们从来都没有调用过这个对象，那么就会浪费内存空间

```java
// 饿汉式单例
public class Hungry {
    private Hungry(){} // 构造器私有
    private static Hungry instance = new Hungry();
    public static Hungry getInstance(){
        return instance;
    }
}
```
### 12.3 懒汉式单例
* 懒加载：我们只有在真正使用一个对象时才会去实例化该对象，而不是程序一启动它就构建好了
```java
/**
 * 懒汉式单例
 *     存在问题：再多线程的情况下，如果一个线程进入了 if (instance == null) 的代码块，还未来得及往下执行，
 *             另一个线程也通过了这个代码块，这时便会产生多个实例
 */
public class LazyMan {
    private LazyMan(){} // 构造器私有
    private static LazyMan instance = null;
    public static LazyMan getInstance(){
        if(instance == null){
            instance = new LazyMan();
        }
        return instance;
    }
}
```
### 12.4 双重锁
```java
// 双重锁
public class Singleton {
    private Singleton(){} // 构造器私有
    private static volatile Singleton instance = null;
    public static Singleton getInstance(){
        if(instance == null){
            synchronized (Singleton.class){
                if(instance == null){
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```
* 如果不加 volatile 关键字，会怎么样？
instance = new Singleton(); 这段代码不是一个原子性操作，它一共分为三步：
（1）分配内存空间
（2）调用构造方法，初始化成员变量
（3）将引用 instance 指向内存地址
如果没有 volatile 关键字，那么在 JVM 在执行时可能会为了效率而进行指令重排，比如：按照 1、3、2 的顺序执行。如果按照这个顺序，那么一个线程执行到第 3 步时，对象还没有被初始化。假设此时另一个线程执行到了 if(instance == null)，那么在该线程中 if(instance == null) 会返回 false，因此会直接跳过该 if，并 return instance，但是此时的 instance 还没有初始化成员变量，那么就会出现问题，因此需要加上 volatile 关键字。

| 线程 A                       | 线程 B                                                       |
| ---------------------------- | ------------------------------------------------------------ |
| 分配内存空间                 |                                                              |
| 将引用 instance 指向内存地址 |                                                              |
|                              | 判断 instance 是否为空                                       |
|                              | 由于 instance 不为 null，线程 B 会得到未初始化的 instance 对象 |
| 初始化对象                   |                                                              |

### 12.5 静态内部类
```java
/**
 * 静态内部类
 *     优点：静态内部类在程序启动的时候不会加载，只有第一次被调用的时候才会加载，因此它是懒汉式的；
 *          没有线程安全问题
 */
public class Holder {
    private Holder(){} // 构造器私有
    // 静态内部类在程序启动的时候不会加载，只有第一次被调用的时候才会加载
    private static class InnerClass{
        private static final Holder INSTANCE = new Holder();
    }
    public static final Holder getInstance(){
        return InnerClass.INSTANCE;
    }
}
```
## 十三、CAS
### 13.1 基础知识
* CAS：Compare And Swap。假设内存中的原数据 V，期望值 A，需要修改的新值 B，当且仅当期望值 A 和内存值 V 相同时，才会将内存值 V 修改为新值 B，否则什么都不做。
```java
// except：期望值，update：更新值
// 如果期望值达到了，那么就更新，否则就不更新
public final boolean compareAndSet(int expect, int update) {
    return unsafe.compareAndSwapInt(this, valueOffset, expect, update);
}
```
```java
import java.util.concurrent.atomic.AtomicInteger;

public class CASTest {
    public static void main(String[] args) {
        AtomicInteger atomicInteger = new AtomicInteger(1);
        
        System.out.println(atomicInteger.compareAndSet(1, 2)); // true
        System.out.println(atomicInteger); // 2

        System.out.println(atomicInteger.compareAndSet(3, 4)); // false
        System.out.println(atomicInteger); // 2
    }
}
```
### 13.2 ABA 问题
* 比如：线程 1 从内存中取出了 A，这时候线程 2 也从内存中取出了 A，并且线程 2 进行了一些操作：将 A 变成了 B，然后又将 B 变成了 A，这时候线程 1 执行 CAS 操作时发现内存中的值仍然是 A，所以线程 1 的 CAS 操作成功，但是不代表这个过程就是没有问题，因为此时的 A 已经发生了变化，不是原来的 A 了
* 如何解决 ABA 问题？
原子引用，即带**版本号**的原子操作
```java
import java.util.concurrent.atomic.AtomicStampedReference;

public class CASTest02 {
    public static void main(String[] args) {
        // 参数：初始值，版本号
        AtomicStampedReference<Integer> atomicStampedReference = new AtomicStampedReference<>(0, 1);
        new Thread(() -> {
            int stamp = atomicStampedReference.getStamp(); // 获得版本号
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            // 参数：期望的值，新的值，期望的版本号，新的版本号
            System.out.println(atomicStampedReference.compareAndSet(0, 1, atomicStampedReference.getStamp(), atomicStampedReference.getStamp() + 1));
            System.out.println(Thread.currentThread().getName() + " -> " + atomicStampedReference.getStamp());
            
            System.out.println(atomicStampedReference.compareAndSet(1, 0, atomicStampedReference.getStamp(), atomicStampedReference.getStamp() + 1));
            System.out.println(Thread.currentThread().getName() + " -> " + atomicStampedReference.getStamp());
        }, "A").start();
        
        new Thread(() -> {
            int stamp = atomicStampedReference.getStamp(); // 获得版本号
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(atomicStampedReference.compareAndSet(0, 1, stamp, stamp + 1));

            System.out.println(Thread.currentThread().getName() + " -> " + atomicStampedReference.getStamp());
        }, "B").start();
    }
}
```

## 十四、其它
### 14.1 基本的锁的概念
* 公平锁：多个线程按照申请锁的顺序去获得锁，线程会直接进入队列去排队，永远都是队列的第一位才能得到锁。**公平锁会保证先到的线程优先获取锁。**
优点：所有的线程都能得到资源，不会饿死在队列中。
缺点：吞吐量会下降很多，队列里面除了第一个线程，其他的线程都会阻塞，CPU 唤醒阻塞线程的开销会很大。
* 非公平锁：多个线程去获取锁的时候，会直接去尝试获取，如果能获取到，就直接获取到锁；如果获取不到，再去进入等待队列。
优点：可以减少 CPU 唤醒线程的开销，整体的吞吐效率会高点，CPU也不必取唤醒所有线程，会减少唤起线程的数量。
缺点：可能导致队列中的线程一直获取不到锁，导致饿死。
* 可重入锁：某个线程已经获得了某个锁，那么它可以再次获取该锁而不会出现死锁。
比如：一个线程已经获得了对象 A 上的锁，那么它第二次请求对象 A 的锁时必然也可以获得（也就是说不会自己把自己锁住） 
### 14.2 死锁
```java
public class DeathLockTest {
    public static void main(String[] args) {
        Object o1 = new Object();
        Object o2 = new Object();
        new Thread(new MyThread(o1, o2)).start();
        new Thread(new MyThread(o2, o1)).start();
    }
}

class MyThread implements Runnable{
    Object o1;
    Object o2;
    public MyThread(Object o1, Object o2){
        this.o1 = o1;
        this.o2 = o2;
    }
    @Override
    public void run() {
        synchronized (o1){
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (o2){
                System.out.println("111");
            }
        }
    }
}
```
* 如何排查死锁？
（1）在 terminal 窗口里使用 ``jps -l``，来定位进程号。
![ts](https://img-blog.csdnimg.cn/df232ba952f54e8b8731d8cb9a2c3ca3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
（2）使用 ``jstack 进程号`` 查看**堆栈信息**，找到死锁问题。
![ts](https://img-blog.csdnimg.cn/9a3b67d3b22a4a2880ef880540ab0d9e.png)
![ts](https://img-blog.csdnimg.cn/fb175ac7549348de8c61cfb553ab787a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)