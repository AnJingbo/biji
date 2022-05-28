

## BIO
* Java 的 BIO 就是传统的 I/O 编程，其相关的类和接口都在 java.io 包下
* BIO（blocking I/O）：同步阻塞，如果想要一个服务端就可以处理多个客户端的连接请求，那么服务端需要为每个客户端请求都建立一个线程（即每个客户端的连接请求，服务端都需要创建一个线程进行处理），可以通过线程池的机制来改善
### BIO 的文件输入输出
```java
public class IOTest1 {
    /**
     * 最好不要用 throws 的方式。因为如果把字符流创建好了之后，在 read() 操作
     * 的时候抛出异常导致程序终止了，那么就会导致这个字符流没有 close() 关闭掉
     */
    @Test
    public void test1() throws IOException {
        FileReader fr = new FileReader("E:\\test.txt");
        int data;
        while ((data = fr.read()) != -1) {
            System.out.print((char) data);
        }
        fr.close();
    }

    @Test
    public void test2() {
        FileReader fr = null;
        try {
            fr = new FileReader("E:\\test.txt");
            int data;
            while ((data = fr.read()) != -1) {
                System.out.print((char) data);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fr != null) {
                try {
                    fr.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
    @Test
    public void test3() {
        FileReader fr = null;
        try {
            fr = new FileReader("E:\\test.txt");
            char[] buffer = new char[4];
            int len;
            while ((len = fr.read(buffer)) != -1) {
                for(int i = 0; i < len; i++){
                    System.out.print(buffer[i]);
                }
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fr != null) {
                try {
                    fr.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
```java
public class IOTest2 {
    @Test
    public void test1() {
        FileWriter fw = null;
        try {
            // 默认会覆盖原文件中的所有内容。比如原文件中的内容为：xyzhhh，写入了 abc 后，文件中的内容会变为：abc
            fw = new FileWriter("E:\\test.txt");
            
            // 在原文件的基础上追加内容
            // fw = new FileWriter("E:\\test.txt", true);
            fw.write("你叫什么名字\n");
            fw.write("我也不知道");
            fw.flush();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fw != null) {
                try {
                    fw.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
```java
public class IOTest3 {
    // 缓冲流的作用：提高流的读取、写入的速度
    // 原理：内部提供了缓冲区
    @Test
    public void test1() {
        FileInputStream fis = null;
        FileOutputStream fos = null;
        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;
        try {
            fis = new FileInputStream("E:\\test.txt");
            fos = new FileOutputStream("D:\\test.txt");
            bis = new BufferedInputStream(fis);
            bos = new BufferedOutputStream(fos);

            byte[] buffer = new byte[4];
            int len;
            while ((len = bis.read(buffer)) != -1) {
                bos.write(buffer, 0, len);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // 先关闭外层的流（bis、bos），再关闭内层的流（fis、fos）
            // 在关闭外层流的时候，会自动将内层的流也一起关闭，所以内层流的关闭可以省略
            if (bis != null) {
                try {
                    bis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (bos != null) {
                try {
                    bos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
* RandomAccessFile（随机文件操作）：它的功能丰富，可以从文件的任意位置进行存取（输入输出）操作。
```java
public class IOTest4 {
    @Test
    public void test1() {
        RandomAccessFile raf1 = null;
        RandomAccessFile raf2 = null;
        try {
            raf1 = new RandomAccessFile("E:\\test.txt", "r");
            raf2 = new RandomAccessFile("D:\\test.txt", "rw");
            byte[] buffer = new byte[4];
            int len;
            while ((len = raf1.read(buffer)) != -1) {
                raf2.write(buffer, 0, len);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (raf1 != null) {
                try {
                    raf1.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (raf2 != null) {
                try {
                    raf2.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
    @Test
    public void test2() {
        RandomAccessFile raf = null;
        try {
            raf = new RandomAccessFile("E:\\test.txt", "rw");
            // 不会覆盖原文件的全部内容。比如原文件中的内容为：xyzhhh，写入了 abc 后，文件中的内容会变为：abchhh
            raf.write("abc".getBytes());

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (raf != null) {
                try {
                    raf.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
    @Test
    public void test3() {
        RandomAccessFile raf = null;
        try {
            raf = new RandomAccessFile("E:\\test.txt", "rw");
            raf.seek(3); // 从下标为 3 的地方开始写
            // 比如原文件中的内容为：xyzhhh，从下标为 3 的地方开始写，写入 abc 后，文件中的内容会变为：xyzabc
            raf.write("abc".getBytes());

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (raf != null) {
                try {
                    raf.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
### BIO 的网络通信
```java
public class Client {
    public static void main(String[] args) throws IOException {
        // 创建 socket 对象，并将其连接到指定 IP 地址的指定端口号
        Socket socket = new Socket("localhost", 9999);
        // 获得字节输出流
        OutputStream os = socket.getOutputStream();
        os.write("hello world!".getBytes());
        os.flush();

        os.close();
        socket.close();
    }
}

public class Server {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(9999);
        // 监听 9999 端口，等待客户端连接。accept() 方法：如果没有客户端连接则会一直阻塞
        Socket socket = serverSocket.accept();

        // 获得字节输入流
        InputStream is = socket.getInputStream();
        byte[] buffer = new byte[1024];
        int len;
        System.out.println("等待数据的输入");
        // read() 方法：如果没有读取到数据的话，那么会一直阻塞，直到有可供读取的数据
        while ((len = is.read(buffer)) != -1) {
            System.out.println(new String(buffer, 0, len));
        }

        is.close();
        socket.close();
        serverSocket.close();
    }
}
```
```java
public class Client1 {
    public static void main(String[] args) throws IOException {
        // 创建 socket 对象，并将其连接到指定 IP 地址的指定端口号
        Socket socket = new Socket("localhost", 9999);
        // 获得字节输出流
        OutputStream os = socket.getOutputStream();
        os.write("hello world!".getBytes());
        os.flush();

        os.close();
        socket.close();
    }
}

public class Server1 {
    /**
     * 一个服务器处理多个客户端的 Socket 连接。
     * 以下的处理方式是有缺陷的：假设一个客户端连接到服务器后，一直不发送数据，那么
     * 服务端就会一直阻塞在 read() 方法处等待客户端的数据，从而导致该服务器无法处理
     * 其它客户端的请求；或者如果一个客户端要发送的数据有很多，那么服务端就需要一直读取数据
     * 直到这个客户端发送完数据并且断开连接后，服务端才能再次调用 accept() 方法来等待下一个客户端的连接
     */
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(9999);
        while(true){
            // 监听 9999 端口，等待客户端连接。accept() 方法：如果没有客户端连接则会一直阻塞
            Socket socket = serverSocket.accept();
            // 获得字节输入流
            InputStream is = socket.getInputStream();
            byte[] buffer = new byte[1024];
            int len;
            // read() 方法：如果没有读取到数据的话，那么会一直阻塞，直到有可供读取的数据
            while ((len = is.read(buffer)) != -1) {
                System.out.println(new String(buffer, 0, len));
            }
        }
    }
}
```
```java
public class Client2 {
    public static void main(String[] args) throws IOException {
        // 创建 socket 对象，并将其连接到指定 IP 地址的指定端口号
        Socket socket = new Socket("localhost", 9999);
        // 获得字节输出流
        OutputStream os = socket.getOutputStream();
        while(true){
            Scanner sc = new Scanner(System.in);
            String msg = sc.nextLine();
            os.write(msg.getBytes());
            os.flush();
        }
    }
}

public class Server2 {
    /**
     * 实现一个服务器处理多个客户端的 Socket 连接。
     * 方法：服务端每收到一个客户端的 Socket 请求后都交给一个单独的线程来处理客户端的数据交互需求
     */
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(9999);
        while(true){
            // 监听 9999 端口，等待客户端连接。accept() 方法：如果没有客户端连接则会一直阻塞
            Socket socket = serverSocket.accept();

            // 创建一个独立的线程来处理与客户端的通信需求
            new ServerThread(socket).start();

        }
    }
}
class ServerThread extends Thread{
    private Socket socket;
    public ServerThread(Socket socket){
        this.socket = socket;
    }
    @Override
    public void run() {
        // 获得字节输入流
        InputStream is = null;
        try {
            is = socket.getInputStream();
            byte[] buffer = new byte[1024];
            int len;
            // read() 方法：如果没有读取到数据的话，那么会一直阻塞，直到有可供读取的数据
            while ((len = is.read(buffer)) != -1) {
                System.out.println(new String(buffer, 0, len));
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
```java
public class Client3 {
    public static void main(String[] args) throws IOException {
        // 创建 socket 对象，并将其连接到指定 IP 地址的指定端口号
        Socket socket = new Socket("localhost", 9999);
        // 获得字节输出流
        OutputStream os = socket.getOutputStream();
        while(true){
            Scanner sc = new Scanner(System.in);
            String msg = sc.nextLine();
            os.write(msg.getBytes());
            os.flush();
        }
    }
}

public class Server3 {
    /**
     * 实现一个服务器处理多个客户端的 Socket 连接。
     * 方法：服务端每收到一个客户端的 Socket 请求后都交给一个单独的线程来处理客户端的数据交互需求
     */
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(9999);
        ServerThreadPool pool = new ServerThreadPool();
        while(true){
            // 监听 9999 端口，等待客户端连接。accept() 方法：如果没有客户端连接则会一直阻塞
            Socket socket = serverSocket.accept();

            pool.execute(new ServerThread(socket));
        }
    }
}

class ServerThreadPool{
    private ExecutorService executorService;
    public ServerThreadPool(){
        executorService = new ThreadPoolExecutor(3, 8,
                60, TimeUnit.SECONDS, new ArrayBlockingQueue<Runnable>(3));
    }

    public void execute(Runnable target){
        executorService.execute(target);
    }
}
class ServerThread implements Runnable{
    private Socket socket;
    public ServerThread(Socket socket){
        this.socket = socket;
    }
    @Override
    public void run() {
        // 获得字节输入流
        InputStream is = null;
        try {
            is = socket.getInputStream();
            byte[] buffer = new byte[1024];
            int len;
            // read() 方法：如果没有读取到数据的话，那么会一直阻塞，直到有可供读取的数据
            while ((len = is.read(buffer)) != -1) {
                System.out.println(new String(buffer, 0, len));
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
```java
public class Client4 {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket("localhost", 8888);
        OutputStream os = socket.getOutputStream();

        Scanner sc = new Scanner(System.in);
        String str = sc.next();
        os.write(str.getBytes());
        
        try {
            Thread.sleep(10000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        os.flush();
        os.close();
        socket.close();
    }
}
public class Server {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(8888);
        while(true){
            System.out.println("执行！");
            Socket socket = serverSocket.accept();
            InputStream is = socket.getInputStream();
            byte[] buffer = new byte[1024];
            int len;
            // read() 方法：如果没有读取到数据的话，那么会一直阻塞，直到有可供读取的数据
            while ((len = is.read(buffer)) != -1) {
                System.out.println(new String(buffer, 0, len));
            }
        }
    }
}
```

### BIO 的缺点

1. 一个服务端的线程只能单独处理一个客户端的请求，这样可能会导致服务端的线程数过多，增大服务器开销（比如：线程之间的上下文切换、线程本身也会占用栈空间和 CPU 资源）。如果访问量过大，系统将发生线程栈溢出，使得线程创建失败，最终导致宕机从而不能为外部提供服务（可以通过线程池的机制来改善）
2. 如果某个客户端的连接请求来了，服务端也创建了一个线程去进行处理客和服之间的通信，但是如果之后这个连接不做任何事情（客户端并不是每时每刻都会发送消息，如果客户端有一段时间没有发送消息，那么其对应的线程就会一直阻塞，导致会有大量的空闲线程），那么就会导致不必要的线程开销
## NIO
* NIO：New IO 或者 Non-blocking IO，可以替代传统的 BIO。NIO 与原来的 IO 有同样的作用和目的，但是使用的方式完全不同。**NIO 支持面向缓冲区、基于通道的 IO 操作**。NIO 将以更加高效的方式进行文件的读写操作。NIO 可以理解为非阻塞 IO，传统的 IO 的 read 和 write 只能阻塞执行，线程在读写 IO 期间不能干其它事情，比如服务端的某个线程在 read() 数据时，如果客户端一直没有数据传输过来，那么线程就一直阻塞
* NIO 的相关类都放在 java.nio 包及子包下，并且对原来的 java.io 包中的很多类进行了改写
* NIO 有三大核心部分：**Channel（通道）、Buffer（缓冲区）和 Selector（选择器）**
* 数据总是从 Channel（通道）中读取到缓冲区，或者从 Buffer（缓冲区）写入到通道中，Selector（选择器）用于监听多个通道的事件（比如：连接请求、数据到达等），因此使用单个线程就可以监听多个客户端通道
* NIO 是可以做到用一个线程来处理多个操作的。假设有 1000 个请求过来，根据实际情况，可以分配 20 到 80 个线程来处理。不用像之前的阻塞 IO 那样，必须得分配 1000 个
### BIO 和 NIO 的比较
* BIO 以流的方式处理数据；而 NIO 以块的方式处理数据。块 I/O 的效率比流 I/O 高很多
* BIO 是阻塞的；而 NIO 是非阻塞的
* BIO 基于字节流和字符流进行操作；而 NIO 基于 Channel（通道）和 Buffer（缓冲区）和 Selector（选择器）进行操作
### NIO 的三大核心原理
* NIO 有三大核心部分：**Channel（通道）、Buffer（缓冲区）和 Selector（选择器）**
* Buffer 缓冲区：缓冲区本质上是一块可以写入数据、并且可以从中读取数据的内存，这块内存被包装成 NIO Buffer 对象，并提供了一组方法，用来方便地访问该块内存
* Chnnel 通道：Java NIO 的通道类似流，但又有些不同：
（1）既可以从通道中读取数据，又可以写入数据到通道；但流的读写通常是单向的（input 或 output）
（2）通道中的数据总是要先读取到一个缓冲区，或者总是要从一个缓冲区中写入数据到通道
（3）通道可以实现异步读写数据
* Selector 选择器：Selector 是一个 Java NIO 的组件，可以检查一个或多个 Channel 通道，并确定那些通道已经准备好读取或写入。这样，一个单独的线程就可以管理多个 Channel 通道，从而管理多个网络连接，提高效率
![ts](https://img-blog.csdnimg.cn/aaa72ee630484aca841ce39e528ef3f3.png?x-oss-process=imagewatermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_16,color_FFFFFF,t_70,g_se,x_16)
* 每个 channel 都会对应一个 buffer
* 一个线程对应一个 Selector，一个 Selector 对应多个 channel
* 程序切换到哪个 channel 是由事件决定的，比如读事件、写事件等。Selector 会根据不同的事件，在各个通道上进行切换
* Buffer 就是一个内存块，底层是一个数组。数据的读取和写入是通过 Buffer 来完成的。BIO 中要么是输入流，要么是输出流，不能双向，但是 NIO 的 Buffer 既可以读也可以写
### Buffer 缓冲区
* Buffer 的底层其实就是数组
* Buffer 中的重要概念：
（1）**容量（capacity）**：作为一个内存块，Buffer 具有固定的大小，也称为容量。缓冲区容量不能为负，而且创建后不能更改
（2）**限制（limit）**：表示缓冲区中可以操作数据的大小（limit 之后的数据不能进行读写）。缓冲区的限制不能为负，并且不能大于其容量。**写入模式：限制等于 Buffer 的容量；读取模式：limit 等于已经写入的数据量**
（3）**位置（position）**：下一个要读取或写入的数据的索引。缓冲区的位置不能为负，并且不能大于其限制
（4）**标记（mark）与重置（reset）**：标记是一个索引，通过 Buffer 的 mark() 方法指定 Buffer 中一个特定的 position，之后可以通过调用 reset() 方法恢复到这个 position
（5）标记、位置、限制、容量遵循以下不变式：0 <= mark <= position <= limit <= capacity
#### 直接和非直接缓冲区
* ByteBuffer 可以是两种类型，一种是基于**直接内存（也就是非堆内存）**；另一种是**非直接内存（也就是堆内存）**。对于直接内存来说，JVM 将会在 IO 操作上具有更高的性能，因为它直接作用于本地系统的 IO 操作。而非直接内存如果要做 IO 操作，会先从本进程内存复制到直接内存，再利用本地 IO 处理
* 直接内存使用 `allocateDirect` 创建，但是它比申请普通的堆内存需要耗费更高的性能。字节缓冲区使用的是否是直接缓冲区，可以调用 `isDirect()` 方法来确定
* 何时使用直接内存：
（1）有很大的数据需要存储，而且它的生命周期又很长
（2）适合频繁的 IO 操作，比如网络并发场景
#### 常用方法
```java
public class BufferTest {
    @Test
    public void test1() {
        // 分配一个容量为 10 的缓冲区（Buffer 的底层就是一个数组）
        ByteBuffer buffer = ByteBuffer.allocate(10);
        System.out.println(buffer.position()); // 0
        System.out.println(buffer.limit()); // 10
        System.out.println(buffer.capacity()); // 10

        // 向缓冲区中添加数据。put() 方法会移动 position 指针
        buffer.put("abcdef".getBytes());
        System.out.println(buffer.position()); // 6 注：索引 6 就是第 7 个位置
        System.out.println(buffer.limit()); // 10
        System.out.println(buffer.capacity()); // 10

        // flip()：将缓冲区的界限(limit)设置为当前位置(position)，然后将当前位置(position)设为 0
        buffer.flip();
        System.out.println(buffer.position()); // 0
        System.out.println(buffer.limit()); // 6
        System.out.println(buffer.capacity()); // 10

        // get() 方法进行数据的读取
        char c = (char)buffer.get();
        System.out.println(c); // a
        System.out.println(buffer.position()); // 1
        System.out.println(buffer.limit()); // 6
        System.out.println(buffer.capacity()); // 10
    }
    @Test
    public void test2() {
        // 分配一个容量为 10 的缓冲区（Buffer 的底层就是一个数组）
        ByteBuffer buffer = ByteBuffer.allocate(10);
        System.out.println(buffer.position()); // 0
        System.out.println(buffer.limit()); // 10
        System.out.println(buffer.capacity()); // 10

        // 向缓冲区中添加数据。put() 方法会移动 position 指针
        buffer.put("abcdef".getBytes());
        System.out.println(buffer.position()); // 6 注：索引 6 就是第 7 个位置
        System.out.println(buffer.limit()); // 10
        System.out.println(buffer.capacity()); // 10

        // clear()：清除缓冲区。位置(position)设置为零，限制(limit)设置为容量
        // clear() 方法只会设置相关变量的值，而不会清除缓冲区中的数据
        buffer.clear();
        System.out.println(buffer.position()); // 0
        System.out.println(buffer.limit()); // 10
        System.out.println(buffer.capacity()); // 10
        System.out.println((char)buffer.get()); // a
    }
    @Test
    public void test3() {
        // 分配一个容量为 10 的缓冲区
        ByteBuffer buffer = ByteBuffer.allocate(10);

        // 向缓冲区中添加数据
        buffer.put("abcdef".getBytes());

        // flip()：将缓冲区的界限(limit)设置为当前位置(position)，然后将当前位置(position)设为 0
        buffer.flip();

        byte[] b1 = new byte[2];
        buffer.get(b1);
        System.out.println(new String(b1)); // ab
        System.out.println(buffer.position()); // 2
        System.out.println(buffer.limit()); // 6
        System.out.println(buffer.capacity()); // 10

        // 标记此刻缓冲区中的位置(position)：2
        buffer.mark();

        byte[] b2 = new byte[3];
        buffer.get(b2);
        System.out.println(new String(b2)); // cde
        System.out.println(buffer.position()); // 5
        System.out.println(buffer.limit()); // 6
        System.out.println(buffer.capacity()); // 10

        // 回到之前标记的位置
        buffer.reset();
        // hasRemaining()：告诉当前位置(position)和限制(limit)之间是否存在任何元素。
        if(buffer.hasRemaining()){
            // 返回当前位置(position)和限制(limit)之间的元素数量。
            System.out.println(buffer.remaining()); // 4
        }
    }
    @Test
    public void test4() {
        // 分配一个容量为 10 的非直接缓冲区（堆内存）
        ByteBuffer buffer = ByteBuffer.allocate(10);
        System.out.println(buffer.isDirect()); // false

        // 分配一个容量为 10 的直接缓冲区（非堆内存）
        ByteBuffer buffer1 = ByteBuffer.allocateDirect(10);
        System.out.println(buffer1.isDirect()); // true
    }
}
```
### Channel 通道
* Channel 本身不能直接访问数据，Channel 只能与 Buffer 进行交互
* Java NIO 的 Channel 通道类似流，但又有些不同：
（1）既可以从通道中读取数据，又可以写入数据到通道；但流的读写通常是单向的（input 或 output）
（2）通道中的数据总是要先读取到一个缓冲区，或者总是要从一个缓冲区中写入数据到通道
（3）通道可以实现异步读写数据

```java
public class ChannelTest {
    /**
     * 写数据到文件中
     */
    @Test
    public void test1() throws IOException {
        // 定义一个字节输出流，字节输出流通向目标文件
        FileOutputStream fos = new FileOutputStream("D:\\test.txt");
        // FileChannel 是一个连接到文件的通道
        FileChannel channel = fos.getChannel();
        // 分配一个缓冲区
        ByteBuffer buffer = ByteBuffer.allocate(1024);
        // 写入数据到缓冲区
        buffer.put("hello, world! 你好,世界!".getBytes());

        buffer.flip();
        // 将 Buffer 中的数据写入到 Channel
        channel.write(buffer);
        channel.close();
    }
    /**
     * 读数据到内存中
     */
    @Test
    public void test2() throws IOException {
        // 定义一个字节输入流与目标文件接通
        FileInputStream fis = new FileInputStream("D:\\test.txt");
        // FileChannel 是一个连接到文件的通道
        FileChannel channel = fis.getChannel();
        // 分配一个缓冲区
        ByteBuffer buffer = ByteBuffer.allocate(1024);

        // 从 Channel 中读取数据到 Buffer
        channel.read(buffer);
        buffer.flip();
        System.out.println(new String(buffer.array(), 0, buffer.remaining()));
    }

    /**
     * 实现文件的复制
     */
    @Test
    public void copy() throws IOException {
        FileInputStream fis = new FileInputStream("D:\\test.txt");
        FileOutputStream fos = new FileOutputStream("D:\\test1.txt");
        // FileChannel 是连接到文件的通道
        FileChannel fisChannel = fis.getChannel();
        FileChannel fosChannel = fos.getChannel();
        ByteBuffer buffer = ByteBuffer.allocate(1024);

        int len = 0;
        // 从 Channel 中读取数据到 Buffer
        while((len = fisChannel.read(buffer)) != -1){
            buffer.flip();
            // 将 Buffer 中的数据写入到 Channel
            fosChannel.write(buffer);
            buffer.clear();
        }
        fisChannel.close();
        fosChannel.close();
    }
}
```
### Selector 选择器（多路复用器）
* 利用 Selector 可使一个单独的线程管理多个 Channel。Selector 是非阻塞 IO 的核心
* Java 的 NIO，是用非阻塞的 IO 方式，可以用一个线程来处理多个客户端的连接
* 多个 Channel 可以以事件的方式注册到同一个 Selector，Selector 能够检测多个注册的通道上是否有事件发生，如果有事件发生，那么就获取事件然后针对每个事件进行相应的处理。这样就可以只用一个单线程去管理多个通道，也就是管理多个连接请求
* 优点：只有在连接/通道真正有读写事件发生时，才会进行读写，大大减少了系统开销，并且不必为每个连接都创建一个线程，不用去维护多个线程，避免了大量线程上下文切换导致的开销
![ts](https://img-blog.csdnimg.cn/f80d686a7703418daa561cf864680b72.png?x-oss-process=imagewatermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_16,color_FFFFFF,t_70,g_se,x_16)

```java
public class Client {
    public static void main(String[] args) throws IOException {
        SocketChannel socketChannel = SocketChannel.open(new InetSocketAddress(9999));
        socketChannel.configureBlocking(false);
        // socketChannel.connect(new InetSocketAddress(9999));

        ByteBuffer buffer = ByteBuffer.allocate(1024);
        while(true){
            Scanner sc = new Scanner(System.in);
            String msg = sc.nextLine();
            buffer.put(msg.getBytes());
            buffer.flip();
            socketChannel.write(buffer);
            buffer.clear();
        }
    }
}
```
```java
public class Server {
    public static void main(String[] args) throws IOException {
        ServerSocketChannel serverSocketChannel= ServerSocketChannel.open();
        // 切换非阻塞模式
        serverSocketChannel.configureBlocking(false);
        serverSocketChannel.bind(new InetSocketAddress(9999));
        // 获取选择器
        Selector selector = Selector.open();
        // 将通道注册到选择器上，
        serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);

        while(true){
            /**
             * select() 方法的返回值表示发生事件的通道的个数
             * selector.select()：会一直阻塞，直到通道上有事件发生才会返回
             * selector.select(1000)：超时时间为 1 秒。超过了 1 秒还没事件发生，
             *                       程序就会接着往下执行
             */
            int select = selector.select();
            /* 只要走到这里，就说明有通道上产生了事件 */
            // 获取选择器上所有的事件
            Set<SelectionKey> selectionKeys = selector.selectedKeys();
            Iterator<SelectionKey> it = selectionKeys.iterator();
            while(it.hasNext()){
                SelectionKey selectionKey = it.next();
                // 判断该事件是哪一种（比如：连接事件、读时间等）
                if(selectionKey.isAcceptable()){
                    SocketChannel socketChannel = serverSocketChannel.accept();
                    socketChannel.configureBlocking(false);
                    socketChannel.register(selector, SelectionKey.OP_READ);
                }else if(selectionKey.isReadable()){
                    SocketChannel socketChannel = (SocketChannel) selectionKey.channel();
                    ByteBuffer buffer = ByteBuffer.allocate(1024);
                    int len = 0;
                    while((len = socketChannel.read(buffer)) > 0){
                        buffer.flip();
                        System.out.println(new String(buffer.array(), 0, len));
                        buffer.clear();
                    }
                }

                // 处理完成之后需要移除当前事件
                it.remove();
            }
        }
    }
}
```