

### NoSql 数据库概述
* NoSql（**Not Only SQL**），即 “不仅仅是 SQL”，泛指**非关系型数据库。**
* NoSql 不依赖业务逻辑方式存储，而以简单的 **key-value** 模式存储，因此大大增加了数据库的扩展能力。
* NoSql 数据库的特点：
（1）不遵循 Sql 标准。
（2）不支持 ACID。
（3）远超于 SQL 的性能。
* NoSql 适用场景：
（1）对数据高并发的读写。
（2）海量数据的读写。
（3）对数据的高扩展性（数据之间是没有关系的）。
* NoSql 不适用的场景：
（1）需要事务支持。
（2）基于 sql 的结构化查询存储，处理复杂的关系，需要即席查询。
（3）用不着 sql 和用了 sql 也不行的情况，可以考虑用 NoSql
## Redis 概述
### Redis 是什么
* Redis是一个开源的、C 语言编写的、面向键值对（key-value）类型数据的分布式 NoSQL 数据库系统。
* Redis 会周期性地把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了 master-slave（主从）同步
### Redis 使用场景
1、配合关系型数据库做高速缓存
2、基于内存运行并且支持持久化（**持久化的两种方式：RDB 和 AOF**）。内存是断电即失的，因此持久化很重要。
3、定时器、计数器
4、发布、订阅消息系统
* Redis 支持存储的 value 类型很多，包括 **string**（字符串）、**list**（链表）、**set**（集合）、**zset**（sorted set，有序集合）和 **hash**（哈希类型）
### Redis 的特性
1、多样的数据类型
2、持久化
3、集群
4、事务
* Redis 为什么是单线程的？
  单线程指的是网络请求模块使用了一个线程，即一个线程处理所有网络请求，其他模块仍用了多个线程。
  （1） 使用单线程模式的Redis，其开发和维护更简单，因为单线程模式方便开发和调试。
  （2）即使使用单线程模型也能够并发地处理多客户端的请求，主要是因为Redis内部使用了基于epoll的多路复用。
  （3）原因是Redis是基于内存的，它的瓶颈在于机器的内存、网络带宽，而不是CPU，在CPU还没达到瓶颈时机器内存可能就满了、或者带宽达到瓶颈了。因此CPU不是主要原因，那么自然就采用单线程了，况且使用多线程比较麻烦。Redis 基于内存，其本质涉及到的 I/O 读写较少，采用单线程也能达到高性能效果，如果使用多线程，那么还会有线程上下文切换的开销。类似于计算密集型
* 在Redis 4.0 开始就有多线程的概念了，比如 Redis 通过多线程方式在后台删除对象、以及通过 Redis 模块实现的阻塞命令等。
### Redis 基本操作
* redis 的默认端口号是：6379
* 启动 Redis 服务器：``redis-server  /usr/local/bin/redis.conf``（**如果要想开启多个 Redis 服务端，则需要有多个不同的配置文件。一个配置文件对应一个 Redis 服务端**）
* 使用 redis-cli 进行连接：``redis-cli -p 6379``
* 关闭 Redis 服务：``shutdown`` 然后 ``exit``
* redis-benchmark 是一个压力测试工具。测试 100 个并发连接、100000 个请求：``redis-benchmark -h localhost -p 6379 -c 100 -n 100000``
* ``help @数据类型``：查看数据类型对应的相关命令。比如：``help @String``
* Redis 默认有 16 个数据库，默认使用的是第 0 个数据库。可以使用 select 进行数据库的切换。比如切换到第 3 个数据库：``select 3``
* 查看当前数据库的大小：``dbsize``
* 查看数据库所有的 key：``keys *``
* 清除当前的数据库：``flushdb``
* 清除所有的数据库：``flushall``
* 查看的 key 是否存在：``exists key``
* 删除已存在的键（不存在的 key 会被忽略）：``del key``
* 移除 key 到第 1 个数据库：``move key 1``
* 设置 key 的过期时间（单位：秒）：``expire key 10``
* 查看当前 key 的剩余时间：``ttl key``
* 查看当前 key 的类型：``type key``
### Redis 五大基本数据类型
#### String（字符串）
* key 对应的是一个字符串
* 设置键值：``set key value``。如果 key 已经存在，那么会覆盖原来的值
* 获取值：``get key``
* 同时设置多个键值：``mset k1 v1 k2 v2 k3 v3``
* 同时获取多个值：``mget k1 k2 k3``
* 设置键值：``setnx key value``。如果 key 已经存在，那么不会设值成功
* 同时设置多个键值：``msetnx k1 v1 k4 v4`` 这是一个原子性操作，要么全部成功，要么全部失败
* 设置键值（包括过期时间）：``setex key 30  "hello"``
* 向键对应的值追加字符串：``append key "hello"``。如果 key 不存在，相当于 set key "hello"
* 获取值的长度：``strlen key``
* 将 key 中存储的数字值加 1：``incr key``
将 key 中存储的数字值加 3：``incrby key 3``
* 将 key 中存储的数字值减 1：``decr key``
将 key 中存储的数字值减 3：``decrby key 3``
* 截取下标 [0, 3] 的值：``getrange key 0 3``
获取全部的值（和get key是一样的）：``getrange key 0 -1``
* 将下标 0 的值变为 ab：``setrange key 0 ab``
#### List
* key 对应的是一个 List
* 向列表左侧头部添加数据：``lpush mylist "hello1"``
* 向列表左侧头部添加数据：``rpush mylist "hello1"``
* 查看 List 中的所有元素：``lrange mylist 0 -1``
* 查看 List 中下标 [0, 1] 内的元素：``lrange mylist 0 1``
* 移除左侧头部的第一个元素：``lpop mylist``。
* 移除右侧头部的第一个元素：``rpop mylist``
* 查看 List 中下标为 0 的值：``lindex mylist 0``
* 查看 List 的长度：``llen mylist``
* 移除 List 中 1 个值为 "hello1" 的元素：``lrem mylist 1 "hello1"``
移除 List 中 2 个值为 "hello1" 的元素：``lrem mylist 2 "hello1"``
截取 List 中下标 [0, 2] 的元素（即让 List 中只剩下下标为 [0, 2] 内的元素）：``ltrim mylist 0 2``
* 查看是否存在某个 List：``exists list``
*  将 List 中指定下标的值替换为另外一个值（如果 List 不存在或者指定下标没有元素，会报错）：``lset mylist 0 "hello"``
* 在 mylist 列表中的 "hello" 前面插入 "world"：``linsert mylist before "hello" "world"``
在 mylist 列表中的 "hello" 后面插入 "world"：``linsert mylist after  "hello" "world"``
#### Set
* key 对应的是一个 Set
* ``sadd <key> <value1> <value2> ... ``：将一个或多个 member 元素加入到 key 中
* ``smembers <key>``：取出 key 对应的所有值。
* ``sismember <key><value>``：判断集合中的 key 对应所有值中是否含有 value 这个值
* ``scard <key>``：返回 key 对应的值的个数。
* ``srem <key><value1><value2> ....``：删除 key 中对应的某个值
* ``spop <key>``：随机从 key 对应的值中吐出（删除）一个值。
* ``srandmember <key> <n>``：随机从 key 中取出 n 个值。不会从集合中删除 。
* ``smove <source> <destination> <value>``：把 source 中的一个 value 移动到 destination
* ``sinter <key1> <key2>``：返回 key1 和 key2 值中的交集元素
* ``sunion <key1> <key2>``：返回 key1 和 key2 值中的并集元素。
* ``sdiff <key1> <key2>``：返回 key1 和 key2 值中的差集元素(key1 中有，但 key2 中没有的)
### Hash
* 注：这种数据类型的 key 对应的是键值对（field value），类似 Java 中的：Map<String, Map<Object, Object>>
* ``hset <key> <field> <value>``：给 key 集合中的 field 键赋值 value
* ``hget <key> <field>``：从 key 中的 field 取出 value 
* ``hmset <key> <field1> <value1> <field2> <value2>...``： 批量设置hash的值
* ``hexists <key> <field>``：查看 key 中是否存在 field 
* ``hkeys <key>``：列出 key 中对应的所有 field
* ``hvals <key>``：列出 key 的所有value
* ``hincrby <key> <field> <increment>``：为 key 中的 field 的值加上 increment
* ``hsetnx <key> <field> <value>``：将 key 中的 field 的值设置为 value ，当且仅当域 field 不存在
### Zset
* ``zadd  <key> <score1> <value1> <score2> <value2>…``：在 key 中加入一个或多个 value 元素及其 score 值
* ``zrange <key> <start> <stop>  [withscores] ``： 返回 key 中，下标在 [start, stop] 之间的元素。带上 withscores，可以让分数和值一起返回（``zrange key 0 -1``：查看 key 中所有 value）
* ``zrangebyscore key min max [withscores] [limit offset count]``：返回 key 中，所有 score 值介于 [min, max] 之间的值。有序集成员按 score 值递增（从小到大）次序排列。 
* ``zrevrangebyscore key max min [withscores] [limit offset count]``               ：同上，改为从大到小排列。 
* ``zincrby <key> <increment> <value>``：为 key 的 score 加上 increment
* ``zrem  <key> <value>``：删除 key 中指定值的元素 
* ``zcount <key> <min> <max>`` 统计 key 中 [min, max] 区间内的元素个数 
* ``zrank <key> <value>``：返回 key 中 value 的排名，从 0 开始。
### 事务
* Redis 的事务的执行顺序：
（1）开启事务。命令：``multi``
（2）命令入队。（把多个命令放入到一个队列里，这些命令不会立即执行，而是会在执行 ``exec`` 命令时再全部执行）
（3）执行事务。命令：``exec``；或者取消事务，命令：``discard``
* Redis 的事务的性质：
（1）Redis 在事务中的所有命令并不会马上被执行，而是在发起执行命令（exec）的时候才会去全部执行（一次性）。Redis 的事务中的所有命令都会**序列化、按顺序**地执行（顺序性）事务在执行的过程中，**不会被其他客户端发送来的命令请求所打断**（排他性）
（2）**Redis 的事务没有隔离级别的概念**。Redis 事务中的所有命令在没有提交（exec）之前，都不会执行，所以也就不存在关系型数据库中经常出现的脏读，不可重复读，幻读等并发操作的问题。
（3）**Redis 的单条命令是保证原子性的，但是 Redis 的事务不保证原子性**。在 Redis 的同一个事务中，如果命令本身的语法没有问题，只是在执行的过程中出错，那么不会影响其他命令的执行。
```bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> setg k3 v3 # 错误的命令
(error) ERR unknown command `setg`, with args beginning with: `k3`, `v3`, 
127.0.0.1:6379(TX)> exec # 提交时所有的命令都不会执行
(error) EXECABORT Transaction discarded because of previous errors.
```
```bash
127.0.0.1:6379> multi 
OK
127.0.0.1:6379(TX)> set k1 "v1"
QUEUED
127.0.0.1:6379(TX)> incr k1 # 因为 k1 对应的值不是数字，所以会执行失败
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> exec # 虽然一条命令报错了，但是其它命令可以正常执行
1) OK
2) (error) ERR value is not an integer or out of range
3) OK
```
### Redis 实现乐观锁
#### 基本概念
* 悲观锁：很悲观，认为什么时候都会出现问题，所以什么时候都需要加锁
* 乐观锁：很乐观，认为什么时候都不会出现问题，所以不会上锁。在更新数据的时候会去判断一下，在此期间是否有人修改过这个数据，如果没修改过则执行成功，否则会执行失败。
（1）获取 version
（2）在更新的时候比较 version
#### Redis 的监控（watch）测试
* Redis 可以使用 watch 来实现乐观锁。
* ``watch key``：监视 key，如果在事务执行之前这个 key 被其他线程所改动，那么事务将被打断。
* ``unwatch``：取消 watch 命令对所有 key 的监视。
* 当执行 exec 或者 discard 命令时，对键的监控（watch）就会被取消掉
```bash
127.0.0.1:6379> set money 100
OK
127.0.0.1:6379> set out 0
OK
127.0.0.1:6379> watch money # 会获取 money 的值，并监视。（此时money的值为100）
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> decrby money 20
QUEUED
127.0.0.1:6379(TX)> incrby out 20
QUEUED
# 此时另一个线程将 money 的值修改为 1000
127.0.0.1:6379(TX)> exec # 会和一开始获取的值进行比对，如果没有发生变化则会执行成功，否则会失败。一开始获取的money值为100，另一个线程修改后money的值变为1000，所以会执行失败
(nil)
```
### Jedis
* 什么是 Jedis？
Jedis 是 Java 官方推荐的 Java 连接开发工具。通过 Jedis 我们可以很方便地使用 Java 代码的方式，对 redis 进行操作。
* 使用 Jedis 时导入 jar 包即可
```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>4.0.1</version>
</dependency>
```
* Jedis 使用示例：
```java
import redis.clients.jedis.Jedis;
import redis.clients.jedis.Transaction;

public class TransactionTest {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("192.168.100.130", 6379);
        jedis.flushDB();
        Transaction multi = jedis.multi(); // 开启事务
        try {
            multi.set("k1", "v1");
            multi.set("k2", "v2");
            // int i = 1 / 0;
            multi.exec(); // 执行事务
        } catch(Exception e) {
            multi.discard(); // 放弃事务
            e.printStackTrace();
        } finally {
            System.out.println(jedis.get("k1"));
            System.out.println(jedis.get("k2"));
            jedis.close();
        }
    }
}
```
* 如果在连接 Linux 的 redis 服务端时报错： ``redis.clients.jedis.exceptions.JedisConnectionException: Failed to create socket``，解决方案：
（1）配置文件中应该注释 127 地址：``# bind 127.0.0.1``
（2）配置文件中应该修改为 no：``protected-mode no``
（3）然后是 Linux 的防火墙需要开启相应的端口（6379）
### Redis 配置文件
#### NETWORK 网络
* ``bind``：默认情况 bind=127.0.0.1 表示只能接受本地的访问请求，在不写 bind 的情况下，可以接受任何 ip 地址的访问。
* ``protected-mode``：保护模式。如果 protected-mode 为 true，那么在没有设定 bind ip 且没有设密码的情况下，Redis 只允许接受本机的响应，如果需要远程连接需要将本机访问保护模式设置为 no。
* ``Port``：指定 redis 运行的端口，默认为 6379
* ``tcp-backlog``：设置 TCP 的 backlog，backlog 其实是一个连接队列，backlog 队列总和=未完成三次握手队列 + 已经完成三次握手队列。
* ``timeout``：一个空闲的客户端维持多少秒会关闭，0 表示关闭该功能，即永不关闭。
* ``tcp-keepalive``：对客户端的一种心跳检测，每隔 n 秒检测一次。单位为秒，如果设置为 0，则不会进行 keepalive 检测，建议设置成 60 
#### GENERAL 通用
* ``daemonize yes``：是否以后台进程的方式运行，默认为 no，需要手动设置为 yes
* ``pidfile /var/run/redis_6379.pid``：如果以后台的方式运行，我们就需要指定一个 pid 文件
* ``loglevel``：指定日志记录级别，Redis总共支持四个级别：debug、verbose、notice、warning，**默认为 notice**
* ``logfile``：日志文件名称
* ``databases 16``：数据库的数量，默认是 16 个数据库
#### SNAPSHOTTING 快照（RDB 设置）
* 注：RDB 是在 SNAPSHOTTING 进行配置。
* ``save 3600 1``：如果在 3600 秒内至少有 1 个 key 进行了修改，那么我们就进行持久化操作
* ``save 300 100``：如果在 300 秒内至少有 100 个 key 进行了修改，那么我们就进行持久化操作
* ``save 60 10000``：如果在 60 秒内至少有 10000 个 key 进行了修改，那么我们就进行持久化操作
* ``stop-writes-on-bgsave-error``：默认值为 yes。如果持久化出现错误，Redis 是否停止接收数据
* ``rdbcompression``：默认值为 yes。对于存储到磁盘中的快照，可以设置是否进行压缩存储
* ``rdbchecksum``：默认值是yes。保存 .rdb 文件的时候，是否进行数据校验
* ``dbfilename``：设置快照的文件名，**默认的文件名为 dump.rdb**
* ``dir ./``：设置 .rdb 文件保存的目录（设置快照文件的存放路径），默认是当前目录（``./ 在 Linux 中表示当前目录``）。使用上面的 ``dbfilename`` 作为保存的文件名。
#### REPLICATION 复制
#### SECURITY 安全
* ``requirepass``：设置 redis 连接密码。比如: ``requirepass 123``  表示 redis 的连接密码为 123。查看 Redis 的密码：``config get requirepass``
#### CLIENTS
* ``maxclients``：设置可以连接上 Redis 的最大客户端数量
#### APPEND ONLY MODE（AOF 设置）
* ``appendonly no``默认是不开启 AOF 模式的。默认是使用 RDB 的方式持久化的
* ``appendfilename``：设置 AOF 持久化文件的名字，**默认的文件名为 appendonly.aof**
* ``appendfsync``：设置 AOF 的持久化策略。no 表示不执行 fsync，由操作系统保证数据同步到磁盘，速度最快；always 表示每次写入都执行 fsync，以保证数据同步到磁盘；everysec 表示每秒执行一次 fsync，可能会导致丢失这 1s 的数据
### Redis 持久化
* 持久化：把内存中的数据写到磁盘中去，防止服务宕机导致内存数据的丢失。
* Redis 是内存数据库，数据保存在内存中。如果不将内存中的数据库状态保存到磁盘，那么一旦 Redis 所在的服务器发生宕机，Redis 数据库里的所有数据也会消失。所以 Redis 提供了持久化的功能
#### RDB（Redis DataBase）
* 默认的持久化方式就是 RDB，可以在配置文件中的 SNAPSHOTTING 来配置 RDB。
##### 什么是 RDB？
RDB 持久化方式是**在指定的时间间隔内将内存中的数据集快照写入磁盘中**，也就是把某个时刻的所有数据生成一个快照，然后写入到二进制文件中，**默认的文件名为 dump.rdb**。**Reids 在启动时会通过加载 dump.rdb 文件来恢复数据**
##### redis 需要执行 RDB 的时候，服务器会执行以下操作：
1. Redis 会单独创建（fork）一个子进程进行持久化
2. 子进程会先将数据写入到一个临时的 RDB 文件
3. 当子进程完成对临时的 RDB 文件的写入时，redis 会用新的 RDB 文件来替换原来旧的 RDB 文件，并将旧的RDB文件删除
在整个过程中，主进程是不进行 I/O 操作的，这就确保了极高的性能。如果需要进行大规模的数据恢复，并且对于数据恢复的完整性不是特别敏感，那么 RDB 方式要比 AOF 的方式更加高效。RDB 的缺点是最后一次持久化后的数据可能会丢失。
##### RDB 持久化的触发机制
* 手动触发：分别对应 save 和 bgsave 命令
（1）save 命令：在执行 save 指令后，会在主进程中进行持久化操作，并且会阻塞当前的 Redis 服务器：在执行 save 命令期间，Redis 不能处理其它命令，直到 RDB 过程完成为止。对于内存比较大的实例会造成长时间阻塞，线上环境不建议使用。
（2）bgsave 命令：在执行 save 指令后，Redis 进程会执行 fork 操作创建子进程，RDB 持久化过程由子进程负责，完成后自动结束。阻塞只发生在 fork 阶段，一般时间很短。基本上 Redis 内部所有的 RDB 操作都是采用 bgsave 命令。
* 自动触发
（1）自动触发是由我们的配置文件来完成的，可以在配置文件中的 SNAPSHOTTING 来配置。**如果满足其中的配置要求，那么会进行一次持久化，生成 rdb 文件**
（2）**在执行 ``flushall`` 命令后**，会进行一次持久化，生成 rdb 文件
（3）**在退出 redis 的时候**，会进行一次持久化，生成 rdb 文件
##### 如何恢复 rdb 文件：
（1）只需要将 rdb 文件放在我们 Redis 的启动目录即可，**Redis 在启动的时候会自动检查 dump.rdb 并恢复其中的数据。**
（2）查看 dump.rdb 需要存放的位置：
```bash
127.0.0.1:6379> config get dir 
1) "dir"
2) "/usr/local/bin" # 如果在这个目录下存在 dump.rdb 文件，那么 Redis 会在启动的时候自动恢复其中的数据
```
* 优点：
（1）生成 rdb 文件的时候，Redis 主进程会 fork() 一个子进程来处理所有的持久化工作，主进程不需要进行任何的磁盘 I/O 操作，因此性能很好
（2）rdb 是一个非常紧凑的二进制文件，它保存了 Redis 在某个时间点上的数据快照。这种文件非常适合用于备份和灾难恢复。
（3）RDB 在恢复大数据集时的速度比 AOF 的恢复速度要快。
* 缺点：
（1）每次保存 RDB 文件的时候，Redis 主进程都要 fork() 出一个子进程，并由子进程来进行实际的持久化工作。 在数据集比较庞大时， fork() 可能会非常耗时，造成服务器在某某毫秒内停止处理客户端，从而严重影响 Redis 的性能。
（2）因为 RDB 文件需要保存整个数据集的状态， 它并不是一个轻松的操作，所以 Redis 会在指定的时间间隔内做一次持久化，比如每隔 5 分钟保存一次 RDB 文件。所以如果还没来得及做持久化工作就发生了故障导致停机， 那么就有可能会丢失这一段时间内的数据
#### AOF（Append Only File）
* 可以在配置文件中的 APPEND ONLY MODE 来配置 AOF。
*  AOF 持久化方式在 redis 中默认是关闭的，需要修改配置文件来开启该方式。
##### 什么是 AOF
* Redis 会将每一个收到的**写命令**追加到 AOF 文件（**默认的文件名为 appendonly.aof**）中，以此达到记录数据库状态的目的。
* 以日志的形式来记录每个写操作，将 Redis 执行过的所有命令记录下来（读操作不记录，只记录写操作），只允许追加文件但不可以改写文件。**Redis 在启动的时候会根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作。**
##### 文件重写
* AOF 的方式可能会带来一个问题：持久化文件可能会变得越来越大。为了压缩 AOF 的持久化文件，Redis 提供了 ``bgrewriteaof`` 命令。当执行该命令时，会 fork 出一个新的进程来进行文件的重写。在重写 AOF 文件时，并没有读取旧的 AOF 文件，而是将整个内存中的数据库内容以命令的方式重写了一个新的 AOF 文件。
* 注：触发重写，可以通过 ``bgrewriteaof`` 命令，也可以通过设置 Redis 配置文件中 ``auto-aof-rewrite-percentage`` 和 ``auto-aof-rewrite-min-size`` 参数的值来确定何时触发。
##### AOF 持久化的触发机制
（1）每修改同步 always：每次发生数据变更都会被立即同步到 AOF 文件。硬盘 I/O 成为性能瓶颈，性能较差但数据完整性较好。
（2）每秒同步 everysec：每秒同步一次，但可能会导致丢失这 1s 的数据
（3）no：依靠操作系统来保证数据同步到磁盘，redis 不会主动同步数据到磁盘。速度最快，但安全性差
* 优点：
（1）AOF 可以更好地保护数据不丢失。一般 AOF 会每隔 1 秒，通过后台线程成型一次 fsync 操作，最多丢失 1 秒钟的数据。
（2）AOF 日志文件以 append-only 的模式写入，所以没有任何磁盘寻址的开销，写入性能比较高
（3）AOF 日志文件即使过大的时候，出现后台重写操作，也不会影响客户端的读写。
（4）AOF 日志文件的命令通过非常可读的方式进行记录，这个特性非常适合做灾难性的误删除的紧急恢复。比如某人不小心用 flushall 命令清空了所有数据，只要这个时候后台 rewrite 还没有发生，那么就可以立即拷贝 AOF 文件，将最后一条 flushall 命令给删了，然后再将该AOF文件放回去，就可以通过恢复机制，自动恢复所有数据
* 缺点：
（1）对于同一份数据来说，AOF 日志文件通常比 RDB 快照文件要更大。
（2）根据所使用的持久化策略，AOF 的速度可能会慢于 RDB
（3）存在 BUG，就是通过 AOF 记录的日志进行数据恢复时，没有恢复一模一样的数据出来
（4）当数据量比较大时，数据恢复会比较慢。
##### 同时开启 RDB 和 AOF
* 在这种情况下，当 Redis 启动的时候会优先载入 AOF 文件来恢复原始的数据。因为在通常情况下，AOF 文件保存的数据集要比 RDB 文件保存的数据集要完整。
* RDB 的数据不实时，同时使用两者时，服务器在启动时也只会找 AOF 文件。那要不要只使用 AOF 呢？建议不要，因为 RDB 更适合用于备份数据库（AOF 在不断变化时不好备份）、快速重启，而且不会有 AOF 可能潜在的 BUG，可以留着作为一个万一的手段
### Redis 发布/订阅
* Redis 发布/订阅（pub/sub）是一种消息通信模式，发布者（pub）发送消息，订阅者（sub）接收消息。
![ts](https://img-blog.csdnimg.cn/d4dc43dd1ab641019e8a6411efa4b90f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* Redis 客户端可以订阅任意数量的频道。
![ts](https://img-blog.csdnimg.cn/efdb0f8019e844b3b22d98332d85223a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)

* Redis 发布订阅命令
![ts](https://img-blog.csdnimg.cn/0a24e860b14842a98ff70720041ffc36.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 示例：
订阅端：
```bash
127.0.0.1:6379> subscribe kuangshenshuo # 订阅一个频道：kuangshenshuo
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "kuangshenshuo"
3) (integer) 1
1) "message"
2) "kuangshenshuo"
3) "hello"
1) "message"
2) "kuangshenshuo"
3) "world"
```
发布端：
```bash
127.0.0.1:6379> publish kuangshenshuo "hello" # 发布者发送消息到频道
(integer) 1
127.0.0.1:6379> publish kuangshenshuo "world" # 发布者发送消息到频道
(integer) 1
```
* 原理：
（1）通过 subscribe 命令订阅某频道后，Redis 服务端里维护了一个字典，**字典的键就是一个个的频道，字典的值是一个链表。链表中保存了所有订阅了这个频道的客户端**。subcribe 命令的关键，就是将客户端添加到给定频道的订阅链表中。
（2）通过 publish 命令向订阅者发送信息，Redis 服务端会使用给定的频道作为键，在字典中查找记录了订阅这个频道的所有客户端的链表，遍历这个链表，将消息发布给所有的订阅者。
（3）pub/sub 从字面上理解就是发布（publish）和订阅（subscribe）。在 Redis 中，你可以设定对某一个键值进行消息发布及消息订阅，当一个键值上进行了消息发布后，所有订阅了它的客户端都会收到相应的消息。这一功能最明显的用法就是用作实时消息系统，比如即时聊天，群聊等功能。
### Redis 主从复制（主从同步）
* **主从复制，是指将一台 Redis 服务器的数据，复制到其他的 Redis 服务器**。前者称为主节点（master/leader），后者称为从节点（slave/follower）
* 数据的复制是单向的，只能由主节点到从节点。**主机负责写操作，从机负责读操作（即读写分离）**
![ts](https://img-blog.csdnimg.cn/33afca0bdcb04c7e995dfbf185021c55.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_16,color_FFFFFF,t_70,g_se,x_16)
* **默认情况下，每台 Redis 服务器都是主节点。**
* 一个主节点可以有多个从节点（或者没有从节点），但一个从节点只能有一个主节点
* 主从复制的作用主要包括：
（1）数据冗余：主从复制实现了数据的热备份，是持久化之外的另一种数据备份方式
（2）故障恢复：当主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复
（3）读写分离：可以用于实现读写分离：主库写、从库读。读写分离不仅可以提高服务器的负载能力，同时可根据需求的变化，改变从库的数量
（4）负载均衡：在主从复制的基础上，配合读写分离，可以由主节点提供写服务，由从节点提供读服务（即写 Redis 数据时应用连接主节点，读 Redis 数据时应用连接从节点），分担服务器负载，尤其是在写少读多的场景下，通过多个从节点分担读负载，可以大大提高 Redis 服务器的并发量。
（5）高可用基石：主从复制还是哨兵模式和集群能够实施的基础，因此说主从复制是 Redis 高可用的基础
* 一般来说，要将 Redis 运用于工程项目中，只使用一台 Redis 是不够的。原因如下：
（1）从结构上，单个 Redis 服务器会发生单点故障，并且一台服务器需要处理所有的请求负载，压力较大
（2）从容量上，单个 Redis 服务器的内容容量有限，就算一台  Redis 服务器的内存容量为 256G，也不能将所有内存用作 Redis 存储内存。一般来说，**单台 Redis 的最大使用内存不应该超过 20G**
* 环境配置：只用配置从库，不用配置主库（因为每个 Redis 服务端默认自己就是一个主库）
（1）查看当前库的信息：``info replication``
```bash
127.0.0.1:6379> info replication # 查看当前库的信息
# Replication
role:master # 角色 master
connected_slaves:0 # 没有从机
master_failover_state:no-failover
master_replid:ec7e30ed3aa162ff88a24140dc10d152a5a099e0
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
```
（2）需要修改的配置文件中的信息：端口号 ``port``、pid 文件的名字 ``pidfile``、日志文件的名字 ``logfile "xxx.log"``、快照的文件名 ``dbfilename``
* 将当前主机变为另一个主机的从机：``slaveof 另一个主机的 ip 另一个主机的的端口号``。注：通过命令的方式只是临时性地配置主从关系，如果从机断开重启了，那么这个从机就会自动变回主机。如果想变成永久的，需要设置配置文件：``replicaof <masterip> <masterport>``，如果主机有密码：``masterauth <master-password>``
将一个从机重新变为主机：``slaveof no one``
* 主从复制的原理：从机（slave）启动成功连接到主机（master）后会发送一个 sync 同步命令，主机接到命令后，会启动给后台的存盘进程，同时收集所有接收到的用于修改数据集命令，在后台进程执行完毕之后，主机将传送整个数据文件到从机，并完成一次完全同步。
（1）全量复制：**将主节点中的所有数据都发送给从节点**。用于初次复制或其它无法进行增量复制的情况，当数据量过大的时候，会造成很大的网络开销。
（2）增量复制：用于处理在主从复制中**因网络中断等原因造成数据丢失场景，当从节点再次连上主节点，如果条件允许，主节点会补发丢失数据给从节点**，因为补发的数据远远小于全量数据，可以有效避免全量复制的过高开销。但需要注意，如果网络中断时间过长，造成主节点没有能够完整地保存中断期间执行的写命令，则无法进行部分复制，仍使用全量复制。
**注：当从机刚连接到主机时，会自动进行一次完全同步（全量复制）**
### 哨兵模式
* 当主机宕机后，我们可以采取手动的方式将一台从机变为主机：可以使用 ``slaveof no one`` 命令将一台从机变为主机，然后手动将其它从机连接到这个新的主机。这种方式费时费力，还会造成一段时间内服务不可用。因此**当主机宕机时，我们可以使用哨兵模式来自动地从从机中选取一台来作为新的主机，然后自动将其它从机连接到这个新的主机上。** Reids 从 2.8 开始正式提供了 Sentinel（哨兵）模式
* 哨兵能够后台监控主机是否故障，如果故障了则会根据投票数自动地将从库变为主库
* 哨兵模式是一种特殊的模式。哨兵是一个独立的进程，作为进程，它会独立运行。其原理是：**哨兵通过发送命令，等待 Redis 服务器的响应，从而监控运行的多个 Redis 服务器。**
![ts](https://img-blog.csdnimg.cn/3d6c9d2509084a69ba1013cf2cf2d4c6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_14,color_FFFFFF,t_70,g_se,x_16)
* 这里的哨兵有两个作用：
（1）通过哨兵发送命令、让 Redis 服务器响应，来监控它们的运行状态，**包括主服务器和从服务器**
（2）当哨兵检测到主机宕机，会自动将一个从机切换成主机，然后通过**发布订阅模式**通知其他的从机，修改配置文件，让它们切换主机
* 然而一个哨兵进程对 Redis 服务器进行监控，可能会出现问题（比如这个哨兵进程死了）。因此我们可以使用多个哨兵来进行监控，各个哨兵之间还会相互进行监控，这样就形成了多哨兵模式。
![ts](https://img-blog.csdnimg.cn/c1ad68b6948d4f398cde2740500fe7e6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 假设主服务器宕机，哨兵 1 先检测到这个结果，系统并不会马上进行 failover（故障转移）过程，因为仅仅是哨兵 1 主观认为主服务器不可用，这个现象称为**主观下线**。当后面的哨兵也检测到主服务器不可用，并且检测到主服务器不可用的哨兵数量达到一定值时，就会对主服务器进行**客观下线**。当主服务器被判断客观下线以后，各个哨兵会进行协商，选举出一个领导者哨兵，并由领导者哨兵对其进行 failover（故障转移）操作：在从机中选取一个作为新的主机，然后通过发布订阅模式，让各个哨兵把自己监控的从服务器自动连接到新的主机上。同时对已经下线的主服务器保持关注，**当已经下线的主服务器重新上线后，将其设置为新的主节点的从节点**
* 示例：比如现在有主服务器 A，它有两个从服务器：B 和 C。如果主服务器 A 宕机后，哨兵就会进行投票，从两个从服务器 B 和 C 中选择一个作为新的主机，比如选取 B 作为新的主机。然后从机 C 也会自动连接到这个新主机 B 上。如果之后服务器 A 又重新连上了，那么这个服务器 A 也会自动变为新的主机的从机。
* 如何启动哨兵：
（1）配置哨兵配置文件 ``sentinel.conf``
```bash
# 哨兵sentinel监控的redis主节点的 ip port 
# master-name：可以自己命名的主节点名字 只能由字母A-z、数字0-9 、这三个字符".-_"组成。
# quorum 当这些quorum个数sentinel哨兵认为master主节点失联 那么这时 客观上认为主节点失联了
# sentinel monitor <master-name> <ip> <redis-port> <quorum>
sentinel monitor myredis 127.0.0.1 6379 1
```
（2）启动哨兵
```bash
redis-sentinel kconfig/sentinel.conf
```
* 优点：
1、哨兵集群，基于主从复制模式，所有的主从配置优点，它都有
2、主从可以切换，故障可以转移，具有高可用性
3、哨兵模式就是主从模式的升级，手动到自动，更加健壮
* 缺点：
1、Redis不好在线扩容的，集群容量一旦到达上限，在线扩容就十分麻烦
2、当主服务器宕机后，从服务器切换成主服务器的那段时间，服务是不能用的

注：用户在请求数据时，会先去 Redis 缓存中去查询是否有该数据，如果缓存中有则直接返回给用户；如果缓存中没有，则会去数据库中查询。如果数据库中有该数据，会把该数据放入缓存中并返回给用户；如果数据库中没有，则会返回空数据给用户
### 缓存穿透
* 缓存穿透是指：用户想要查询一个数据，发现 Redis 内存数据库没有，也就是缓存没有命中，于是向持久层数据库查询，发现也没有。即**用户查询的数据在缓存和数据库中都没有**，于是本次查询失败。
* 当用户很多的时候，缓存都没有命中，于是都去请求持久层数据库，这会给持久层数据库造成很大的压力
* 穿透带来的问题：如果有黑客会对你的系统进行攻击，拿一个不存在的 id 去查询数据，会产生大量的请求到数据库去查询。可能会导致你的数据库由于压力过大而宕掉。针对于这种恶意攻击，由于攻击带过来的大量 key 是不存在的，那么我们采用缓存的方式就会缓存大量不存在 key 的数据，因此这种方案就不合适了。我们完全可以使用布隆过滤器来过滤掉这些 key。
* 解决方案：
（1）在缓存之前加一层布隆过滤器。将所有可能存在的数据哈希到一个足够大的 bitmap 中，当用户想要查询数据时，如果使用布隆过滤器发现这个数据不在集合中，那么就不再进行查询，直接返回；如果存在再查缓存、持久层
（2）当持久层没有命中后，即使返回的空对象 null 也会被缓存层缓存起来，同时会设置一个过期时间，之后再访问这个数据将会从缓存中获取，保护了持久层。但是这种方法会存在两个问题：1、如果空值 null 能够被缓存起来，这意味着缓存层会需要更多的空间来存储更多的键，因为这当中可能会有很多是空值 null 的键；2、即使对空值设置了过期时间，还是会存在缓存层和持久层的数据有一段时间不一致，这对于需要保持一致性的业务会有影响
### 缓存击穿
* 缓存击穿是指：在**高并发**系统中，**当大量的请求同时查询一个 key 时（即这个 key 是热点数据），此时这个 key 正好失效了，那么就会导致大量的请求都打到数据库上面去**，就像在一个屏障上凿开了一个洞。
* 解决方案：
（1）设置热点数据永远不过期
（2）加互斥锁。缓存击穿是多个线程同时去查询数据库的这条数据，那么我们可以在第一个查询数据的请求上使用一个互斥锁来锁住它，其他的线程走到这一步拿不到锁就等着，等第一个线程查询到了数据，然后做缓存。后面的线程进来发现已经有缓存了，就直接走缓存。
### 缓存雪崩
* 由于缓存层承载了大量的请求，有效地保护了持久层。但是**如果缓存层由于某些原因不可用（比如：缓存服务器宕机）或者大量缓存由于超时时间相同在同一时间段失效（大批 key 失效/热点数据失效），导致大量的请求直接到达了持久层，持久层压力过大导致系统雪崩。**
* 解决方案：
（1）redis 高可用。既然 Redis 有可能挂掉，那我多增设几台 Redis 服务器，这样一台挂掉之后其他的还可以继续工作，其实就是**搭建集群**。
（2）缓存数据的过期时间设置成随机的，防止同一时间大量的数据过期
（3）限流降速。在缓存失效后，通过加锁来控制访问数据库的线程数量，降低数据库的压力