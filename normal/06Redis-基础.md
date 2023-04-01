### Redis线程模型

Redis使用单线程模型主要是为了简单，方便

Redis基于Reactor模式开发了自己的网络事件处理器：这个处理器被称为文件事件处理器.

虽然文件事件处理器以单线程方式运行，但通过使用I/O多路复用程序来监听多个套接字，文件事件处理器既实现了高性能的网络通信模型，又可以很好地与Redis服务器中其他同样以单线程方式运行的模块进行对接，这保持了Redis内部单线程设计的简单性。

<img src="http://ww1.sinaimg.cn/large/005RPBObgy1gj6mlv08iqj30yg0bx0su.jpg" alt="undefined" style="zoom: 67%;" />

- 文件事件处理器使用I/O多路复用程序来同时监听多个套接字，并根据套接字目前执行的任务来为套接字关联不同的事件处理器。
- 当被监听的套接字准备好执行连接应答、读取、写入、关闭等操作时，与操作相对应的文件事件就会产生，这时文件事件处理器就会调用套接字之前关联好的事件处理器来处理这些事件。

文件事件是对套接字操作的抽象，每当一个套接字准备好执行连接应答、写入、读取、关闭等操作时，就会产生一个文件事件。因为一个服务器通常会连接多个套接字，所以多个文件事件有可能会并发地出现。I/O多路复用程序负责监听多个套接字，并向文件事件分派器传送那些产生了事件的套接字。文件事件分为AE_READABLE事件（读事件）和AE_WRITABLE事件（写事件）两类。

尽管多个文件事件可能会并发地出现，但I/O多路复用程序总是会将所有产生事件的套接字都放到一个队列里面，然后通过这个队列，以有序、同步、每次一个套接字的方式向文件事件分派器传送套接字。

**I/O多路复用程序的实现**

Redis的I/O多路复用程序的所有功能都是通过包装常见的select、epoll、evport和kqueue这些I/O多路复用函数库来实现的。

优先顺序是evport->epoll->kqueue->select

如果一个套接字又可读又可写的话，那么服务器将先读套接字，后写套接字。

### 为什么Redis那么快

- C语言
- 纯内存操作(引用计数内存回收)
- 非阻塞的 IO 多路复用机制
- 单线程，避免了多线程的频繁上下文切换问题
- 丰富的数据结构(Redis只对包含整数值的字符串对象进行共享)

### Redis 有哪些数据结构

String，Hash，List，Set，Zset

**SDS(简单动态字符串)**

char buf[]；int len；int free；

常数复杂度获取字符串长度；杜绝缓冲区溢出

减少修改字符串时带来的内存重分配次数；二进制安全；兼容部分C字符串函数

**链表**

```C
typedef struct listNode {
    struct listNode * prev;// 前置节点
    struct listNode * next;// 后置节点
    void * value;// 节点的值
}listNode;

typedef struct list {
    listNode * head;// 表头节点
    listNode * tail;// 表尾节点
    unsigned long len; // 链表所包含的节点数量
    void *(*dup)(void *ptr);// 节点值复制函数
    void (*free)(void *ptr); // 节点值释放函数
    int (*match)(void *ptr,void *key);// 节点值对比函数
} list;
```

双端；无环；有表头表尾指针；长度计数器；多态；

**字典**

```c
//哈希表
typedef struct dictht {
    dictEntry **table;// 哈希表数组
    unsigned long size;// 哈希表大小
    unsigned long sizemask;//哈希表大小掩码，用于计算索引值，总是等于size-1
    unsigned long used; // 该哈希表已有节点的数量
} dictht;
//哈希表节点
typedef struct dictEntry {
    void *key;// 键
    union{
        void *val;uint64_tu64;int64_ts64;
    } v;// 值
    struct dictEntry *next; // 指向下个哈希表节点，形成链表
} dictEntry;
typedef struct dict {
    dictType *type;// 类型特定函数
    void *privdata;// 私有数据
    dictht ht[2]; // 哈希表
    in trehashidx; // rehash索引，当rehash不在进行时，值为-1
} dict;
```

一般情况下，字典只使用ht[0]哈希表，ht[1]哈希表只会在对ht[0]哈希表进行rehash时使用。

<img src="http://ww1.sinaimg.cn/large/005RPBObly1gj7rx70443j30vy0hgwfu.jpg" alt="undefined" style="zoom: 50%;" />

```c
hash = dict->type->hashFunction(key);//使用字典设置的哈希函数，计算键key的哈希值
index = hash & dict->ht[x].sizemask;//使用哈希表的sizemask属性和哈希值，计算出索引值
```

Redis的哈希表使用链地址法来解决键冲突，因为是单链表，所以采用头插法效率比较高

rehash：

当哈希表保存的键值对数量太多或者太少时，程序需要对哈希表的大小进行相应的扩展或者收缩。

扩展：

- 目前没有在执行BGSAVE命令或者BGREWRITEAOF命令，并且哈希表的负载因子大于等于1。

- 目前正在执行BGSAVE命令或者BGREWRITEAOF命令，并且哈希表的负载因子大于等于5。

在执行BGSAVE命令或BGREWRITEAOF命令的过程中，Redis需要创建当前服务器进程的子进程，而大多数操作系统都采用写时复制（copy-on-write）技术来优化子进程的使用效率，所以在子进程存在期间，服务器会提高执行扩展操作所需的负载因子，从而尽可能地避免在子进程存在期间进行哈希表扩展操作，这可以避免不必要的内存写入操作，最大限度地节约内存。

收缩：当哈希表的负载因子小于0.1时

渐进式rehash:

rehash动作并不是一次性、集中式地完成的，而是分多次、渐进式地完成的。

- 在渐进式rehash进行期间，字典的删除、查找、更新等操作会在两个哈希表上进行。
- 在渐进式rehash执行期间，新添加到字典的键值对一律会被保存到ht[1]里面，而ht[0]则不再进行任何添加操作，这一措施保证了ht[0]包含的键值对数量会只减不增，并随着rehash操作的执行而最终变成空表。

**跳跃表**

跳跃表（skiplist）是一种有序数据结构，它通过在每个节点中维持多个指向其他节点的指针，从而达到快速访问节点的目的。跳跃表支持平均O(logN)、最坏O(N)复杂度的节点查找，还可以通过顺序性操作来批量处理节点。

```c
//跳跃表节点
typedef struct zskiplistNode {
    struct zskiplistLevel {
        struct zskiplistNode *forward; // 前进指针      
        unsigned int span;  // 跨度
    } level[]; // 层
    struct zskiplistNode *backward;// 后退指针
    double score; // 分值
    robj *obj;// 成员对象
} zskiplistNode;
//跳跃表
typedef struct zskiplist { 
    structz skiplistNode *header, *tail; // 表头节点和表尾节点 
    unsigned long length;// 表中节点的数量
   
    int level; // 表中层数最大的节点的层数
} zskiplist;
```

**整数集合**

```c
typedef struct intset {
    uint32_t encoding;// 编码方式
    uint32_t length;// 集合包含的元素数量
    int8_t contents[];// 保存元素的数组
} intset;
```

升级：每当我们要将一个新元素添加到整数集合里面，并且新元素的类型比整数集合现有所有元素的类型都要长时，整数集合需要先进行升级（upgrade），然后才能将新元素添加到整数集合里面。不支持降级

**压缩列表**

**对象**

```c
typedef struct redisObject {
    unsigned type:4; // 类型
    unsigned encoding:4;  // 编码
    void *ptr;// 指向底层实现数据结构的指针
} robj;
```

<img src="http://ww1.sinaimg.cn/large/005RPBObly1gj7u25bw4ij30gf070dgg.jpg" alt="undefined" style="zoom:67%;" />

对于Redis数据库保存的键值对来说，键总是一个字符串对象，而值则可以是字符串对象、列表对象、哈希对象、集合对象或者有序集合对象的其中一种

<img src="http://ww1.sinaimg.cn/large/005RPBObly1gj7u3n07lpj30uc0dc776.jpg" alt="undefined" style="zoom: 67%;" />

```c
typedef struct zset {
    zskiplist *zsl;
    dict *dict;
} zset;
```

字典中的每个键值对都保存了一个集合元素：字典的键保存了元素的成员，而字典的值则保存了元素的分值。通过这个字典，程序可以用O(1)复杂度查找给定成员的分值

**Hash主要作用是防止重复使用key，提高效率**

### Redis键过期删除策略

- 惰性删除：放任键过期不管，但是每次从键空间中获取键时，都检查取得的键是否过期，如果过期的话，就删除该键；如果没有过期，就返回该键。
- 定期删除：每隔一段时间，程序就对数据库进行一次检查，删除里面的过期键。至于要删除多少过期键，以及要检查多少个数据库，则由算法决定。
- 当前已用内存超过 maxmemory 限定时，触发主动清理策略

数据库主要由dict和expires两个字典构成，dict字典负责保存键值对，expires字典则负责保存键的过期时间。

- 执行SAVE命令或者BGSAVE命令所产生的新RDB文件不会包含已经过期的键。
- 执行BGREWRITEAOF命令所产生的重写AOF文件不会包含已经过期的键。

当一个过期键被删除之后，服务器会追加一条DEL命令到现有AOF文件的末尾，显式地删除过期键。

当主服务器删除一个过期键之后，它会向所有从服务器发送一条DEL命令，显式地删除过期键。

从服务器即使发现过期键也不会自作主张地删除它，而是等待主节点发来DEL命令，这种统一、中心化的过期键删除策略可以保证主从服务器数据的一致性。

### Redis 数据“淘汰”策略？

Redis 内存数据集大小上升到一定大小的时候，就会进行数据淘汰策略。

Redis 提供了 6 种数据淘汰策略：

1. volatile-lru：从已设置过期时间的数据集中挑选最近最少使用的数据淘汰
2. volatile-ttl：从已设置过期时间的数据集中挑选将要过期的数据淘汰
3. volatile-random：从已设置过期时间的数据集中任意选择数据淘汰
4. allkeys-lru：从数据集中挑选最近最少使用的数据淘汰
5. allkeys-random：从数据集中任意选择数据淘汰
6. 【默认策略】no-enviction：禁止驱逐数据

### Redis持久化方式

**RDB持久化**

DB持久化既可以手动执行，也可以根据服务器配置选项定期执行。RDB持久化功能所生成的RDB文件是一个经过压缩的二进制文件，通过该文件可以还原生成RDB文件时的数据库状态

有两个Redis命令可以用于生成RDB文件，一个是SAVE，另一个是BGSAVE。

- SAVE命令会阻塞Redis服务器进程，直到RDB文件创建完毕为止，服务器不能处理任何命令请求
- BGSAVE命令会派生出一个子进程，由子进程负责创建RDB文件，服务器进程（父进程）继续处理命令请求

RDB文件的载入工作是在服务器启动时自动执行的，所以Redis并没有专门用于载入RDB文件的命令，只要Redis服务器在启动时检测到RDB文件存在，它就会自动载入RDB文件。

另外值得一提的是，因为AOF文件的更新频率通常比RDB文件的更新频率高，所以如果服务器开启了AOF持久化功能，那么服务器会优先使用AOF文件来还原数据库状态。

BGREWRITEAOF和BGSAVE两个命令的实际工作都由子进程执行

<img src="http://ww1.sinaimg.cn/large/005RPBObly1gj7v3zb0jtj30hx01aglk.jpg" alt="undefined" style="zoom: 67%;" />

**AOF持久化**

AOF持久化是通过保存Redis服务器所执行的写命令来记录数据库状态的，服务器在启动时，可以通过载入和执行AOF文件中保存的命令来还原服务器关闭之前的数据库状态

AOF持久化功能的实现可以分为命令追加（append）、文件写入、文件同步（sync）三个步骤。

服务器在处理文件事件时可能会执行写命令，使得一些内容被追加到aof_buf缓冲区里面，所以在服务器每次结束一个事件循环之前，它都会调用flushAppendOnlyFile函数，考虑是否需要将aof_buf缓冲区中的内容写入和保存到AOF文件里面。flushAppendOnlyFile函数的行为由服务器配置的appendfsync选项的值来决定

<img src="http://ww1.sinaimg.cn/large/005RPBObly1gj7viwl638j30w0092aba.jpg" alt="undefined" style="zoom:50%;" />

AOF重写：

为了解决AOF文件体积膨胀的问题，Redis提供了AOF文件重写（rewrite）功能。通过该功能，Redis服务器可以创建一个新的AOF文件来替代现有的AOF文件，新旧两个AOF文件所保存的数据库状态相同，但新AOF文件不会包含任何浪费空间的冗余命令，所以新AOF文件的体积通常会比旧AOF文件的体积要小得多。

但实际上，AOF文件重写并不需要对现有的AOF文件进行任何读取、分析或者写入操作，这个功能是通过读取服务器当前的数据库状态来实现的。

Redis将AOF重写程序放到子进程里执行，这样做可以同时达到两个目的：

- 子进程进行AOF重写期间，服务器进程（父进程）可以继续处理命令请求。
- 子进程有服务器进程的数据副本，使用子进程而不是线程，可以避免使用锁的情况下，保证数据的安全性。

为了解决这种数据不一致问题，Redis服务器设置了一个AOF重写缓冲区，这个缓冲区在服务器创建子进程之后开始使用，当Redis服务器执行完一个写命令之后，它会同时将这个写命令发送给AOF缓冲区和AOF重写缓冲区

当子进程完成AOF重写工作之后，它会向父进程发送一个信号，父进程在接到该信号之后，会调用一个信号处理函数，并执行以下工作：

- 将AOF重写缓冲区中的所有内容写入到新AOF文件中
- 对新的AOF文件进行改名，原子地（atomic）覆盖现有的AOF文件，完成新旧两个AOF文件的替换。

### Redis主从复制

在Redis中，用户可以通过执行SLAVEOF命令或者设置slaveof选项，让一个服务器去复制另一个服务器，我们称呼被复制的服务器为主服务器（master），而对主服务器进行复制的服务器则被称为从服务器（slave）

Redis的复制功能分为同步（sync）和命令传播（command propagate）两个操作：

- 同步操作：将从服务器更新至与主服务器一致的状态。SYNC命令是一个非常耗费资源的操作

  <img src="http://ww1.sinaimg.cn/large/005RPBObly1gj8qfwzm8mj309404odfw.jpg" alt="undefined" style="zoom: 67%;" />

  部分重同步：记录复制偏移量

- 命令传播：在主服务器的数据库状态被修改，导致主从不一致时，让主从重新回到一致状态。

  主服务器将自己执行的写命令，即造成主从服务器不一致的写命令，发送给从服务器执行，当从服务器执行了相同的写命令之后，主从服务器将再次回到一致状态。

**心跳检测**

在命令传播阶段，从服务器默认会以每秒一次的频率，向主服务器发送命令：

```shell
REPLCONF ACK <replication_offset>
```

其中replication_offset是从服务器当前的复制偏移量。作用如下：

- 检测主从服务器的网络连接状态。

- 辅助实现min-slaves选项。

- 检测命令丢失

  <img src="http://ww1.sinaimg.cn/large/005RPBObly1gj8qpp8osej30jq02b74e.jpg" alt="undefined" style="zoom: 67%;" />

### Redis高可用Sentinel(哨兵)

由一个或多个Sentinel实例（instance）组成的Sentinel系统（system）可以监视任意多个主服务器，以及这些主服务器属下的所有从服务器，并在被监视的主服务器进入下线状态时，自动将下线主服务器属下的某个从服务器升级为新的主服务器，然后由新的主服务器代替已下线的主服务器继续处理命令请求。

<img src="http://ww1.sinaimg.cn/large/005RPBObly1gj8qxyynd7j30ha0cz74x.jpg" alt="undefined" style="zoom: 67%;" />

假设这时，主服务器server1进入下线状态，那么从服务器server2、server3、server4对主服务器的复制操作将被中止，并且Sentinel系统会察觉到server1已下线

当server1的下线时长超过用户设定的下线时长上限时，Sentinel系统就会对server1执行故障转移操作：

- 首先，Sentinel系统会挑选server1属下的其中一个从服务器，并将其升级为新的主服务器。
- 之后，Sentinel系统会向server1属下的所有从服务器发送新的复制指令，让它们成为新的主服务器的从服务器，当所有从服务器都开始复制新的主服务器时，故障转移操作执行完毕。
- 另外，Sentinel还会继续监视已下线的server1，并在它重新上线时，将它设置为新的主服务器的从服务器。

### Redis高可用Cluster(集群)

Redis集群通过分片的方式来保存数据库中的键值对：集群的整个数据库被分为16384个槽（slot），数据库中的每个键都属于这16384个槽的其中一个，集群中的每个节点可以处理0个或最多16384个槽。当数据库中的16384个槽都有节点在处理时，集群处于上线状态（ok）；相反地，如果数据库中有任何一个槽没有得到处理，那么集群处于下线状态（fail）。

<img src="http://ww1.sinaimg.cn/large/005RPBObgy1gj8ungey7rj30lt0bet9k.jpg" alt="undefined" style="zoom:67%;" />

### Redis分布式锁

**V1.0**

```java
tryLock(){SETNX Key 1    EXPIRE Key Seconds}
release(){DELETE Key}
```

给锁加一个过期时间是为了避免应用在服务重启或者异常导致锁无法释放后，不会出现锁一直无法释放的情况。

问题：

如果执行完第一条命令后应用异常或者重启，锁将无法过期，一种改善方案就是使用Lua脚本（包含SETNX和EXPIRE两条命令），但是如果Redis仅执行了一条命令后crash或者发生主从切换，依然会出现锁没有过期时间，最终导致无法释放。

**V2.0**

Redis 2.6.12版本后SETNX增加过期时间参数

```
tryLock(){SETNX Key 1 Seconds}
release(){DELETE Key}
```

问题：

- C1成功获取到锁，之后C1因为GC或者未知原因导致任务执行过长，最后在锁失效前C1没有主动释放锁
- C2在C1的锁超时后获取到锁开始执行，这时C1和C2都同时在执行，会因重复执行造成数据不一致等未知情况
- C1如果先执行完毕，则会释放C2的锁，此时可能导致另外一个C3进程获取到了锁

**V3.0**

```
tryLock(){SETNX Key UnixTimestamp Seconds}
release(){EVAL(if redis.call("get",KEYS[1]) == ARGV[1] then return redis.call("del",KEYS[1])   else return 0 end)}//LuaScript
```

这个方案通过指定Value为时间戳，并在释放锁的时候检查锁的Value是否为获取锁的Value

问题：

如果在并发极高的场景下，比如抢红包场景，可能存在UnixTimestamp重复问题，另外由于不能保证分布式环境下的物理时钟一致性，也可能存在UnixTimestamp重复问题，只不过极少情况下会遇到。

改进：

Redis 2.6.12后[SET](https://redis.io/commands/set)同样提供了一个NX参数，另外一个优化是使用一个自增的唯一UniqId代替时间戳来规避V3.0提到的时钟问题。这个方案是目前最优的分布式锁方案，但是如果在Redis集群环境下依然存在问题：

新的问题：

由于Redis集群数据同步为异步，假设在Master节点获取到锁后未完成数据同步情况下Master节点crash，此时在新的Master节点依然可以获取锁，所以多个Client同时获取到了锁

**Lua脚本为什么可以保证原子性？**

Redis使用同一个Lua解释器来执行所有命令，同时当lua脚本在执行的时候，不会有其他脚本和命令同时执行

**分布式Redis锁：Redlock**：解决了主从锁不一致问题

单机、哨兵(sentinel)、集群(cluster)均可

允许加锁失败节点限制；遍历所有节点通过EVAL命令执行lua加锁：对节点尝试加锁，抛出连接超时/失败异常，为了防止加锁成功需要解锁，抛出其他异常表示获取锁失败，如果成功获取锁加入集合，如果达到了允许加锁失败节点限制，那么此次加锁失败。

**分布式Redis锁：Redisson**：解决了自动延锁的问题

解决了锁失效时间太短，然后导致任务还没完成就失效，然后被其他线程拿到锁的情况

看门狗：设置一个定时器，设置一个监听器异步执行，如果异步执行成功则延期成功，如果失败则无法延期

**分布式锁要注意的问题**

为了减少等待，锁的粒度要尽量小比如商品ID，锁住的代码行数要尽量少

分布式锁没获取到锁的会循环尝试获取锁