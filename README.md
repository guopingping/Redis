Redis
===============================================================
```
见解：
    对一些高并发，需要频繁更新使用数据库的操作，利用缓存来处理，可以先存到redis集群中来操作，
    即针对：用户群体比较多，很多客户端、大数据量、频发调用数据库，可以利用redis集群，直接存放到内存，
        提高运算速度，减轻数据库的压力，实现原有数据库很难实现的功能；
    类似：淘宝电商平台，排行榜，点赞评论，页面访问统计，接口调用统计等

```

```
Remote Dictionary Server(Redis):key-value存储系统。
Redis：开源的使用ANSI C语言编写，遵循BSD协议，支持网络，可基于内存亦可持久化的日志型，
        Key-Value数据库，并提供多种语言的API。
    被称为数据结构服务器
    完全开源，高性能的key-value数据库

    redis支持数据的持久化，可将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用；
    redis支持key-value，并提供list，set，zset，hash等数据结构的存储；
    redis支持数据的备份，即master-slave模式的数据备份；

```
Redis优势
----------------------------------------------------------------
```
性能极高；
丰富的数据类型；
原子--redis的所有操作都是原子性的，要么成功执行要么失败完全不执行。单个操作是原子性的。
        多个操作也支持事务，即原子性，通过Multi和Exec指令包起来；
丰富的特性--支持publish、subscribe；

```
Redis与其他key-value存储不同
------------------------------------------------------------------
```
Redis：更为复杂的数据结构并提供对他们原子性操作；对程序员透明；
Redis：运行在内存中可以持久化到磁盘，所以在对不同数据集进行高速读写时要权衡内存，因为数据量不能大于
        硬件内存；相比在磁盘上相同的复杂的数据结构，在内存中操作非常简单，可以做很多内部复杂性很强的事情；
        同时，在磁盘格式方面紧凑的以追加的方式产生的，不需要进行随机访问。
```
Redis数据类型
-----------------------------------------------------------------
```
Redis支持五种数据类型：
    string、hash、list、set、zset(sorted set:有序集合)

String：
    key-value    最大存储512MB
Hash:
    键值对的集合 存储对象 可存储2^32-1键值对
List：
    按插入顺序排序 最多可存储2^32-1元素
Set：
    是string类型的无序集合 集合通过哈希表实现的，添加删除查找复杂度都是o(1)
    
    sadd：
        添加一个string元素到key对应的set集合中，成功返回1 如元素已存在 返回0
        如果key对应的set不存在则返回错误；
        sadd key member
    集合中最大的成员数：2^32-1

zset:(sorted set：有序集合)
    和set一样是string类型元素的集合，不允许重复的成员
    不同：每个元素关联一个double类型的分数。redis通过分数为集合成员从小到大排序
    zset的成员是唯一的，但分数可以重复。

    zadd：
        添加元素到集合，元素在集合中存在则更新对应的score
        zadd key score member

```
Redis命令
-----------------------------------------------------------------------
```
$ redis-cli
ping：检测redis服务是否启动

在远程服务上执行命令：
    $ redis-cli -h host -p port -a password

 redis-cli --raw //避免中文乱码

```

Redis键（key）
--------------------------------------------------------------------------
```

COMMAND KEY_NAME

DEL key：key存在时删除；
DUMP key：序列化给定key，并返回被序列化的值；
EXISTS key：检查给定key是否存在；
EXPIRE key seconds：为给定key设置过期时间，秒计；
EXPIREAT key timestamp：时间参数是unix时间戳；
PEXPIRE key milliseconds：设置key的过期时间毫秒计；
PEXPIREAT key milliseconds-timestamp：时间戳毫秒计；
KEYS pattern：查找所有符合给定模式pattern的key；
MOVE key db：将当前数据库的key移动到给定的数据库db当中；
PERSIST key：移除key的过期时间，key将持久保持；
PTTL key：以毫秒为单位返回key的剩余过期时间；
TTL key：秒为单位 返回给定key的剩余生存时间；
RANDOMEY：从当前数据库随机返回一个key；
RENAME key newkey：修改key的名称；
RENAMENX key newkey：晋档newkey不存在时，将key改为newkey；
TYPE key：返回key所存储的值类型；

```
Redis字符串
----------------------------------------------------------------------------------
```
redis：用于管理redis字符串值

SET key value：设置指定key的值；
GET key：获取指定key的值；
GETRANGE key start end：返回key中字符串值得子字符；
GETSET key value：将给定的key的值设为value，返回key的旧值；
GETBIT key offset：对key所存储的字符串值，获取指定偏移量上的位（bit）；
MGET key1[key2...]：获取所有（一个或多个）给定key的值；
SETBIT key offset value：对key所存储的字符串值，设置或清除指定偏移量上的位（bit）；
SETEX key seconds value：将值value关联到key， 并将key的过期时间设为seconds（以秒为单位）；
SETNX key value：只有在key不存在时设置key的值；
SETRANGE key offset value：用value参数覆写给定key所存储的字符串值，从偏移量offset开始；
STRLEN key：返回key所存储的字符串值得长度；
MSET key value[key value...]：同时设置一个或多个key-value对；
MSETNX key value[key value...]：同时设置一个霍多个key-value对，当且仅当所有给定key都不存在；
PSETEX key milliseconds value：以秒为单位设置key的生存时间；
INCR key：将key中存储的数字值增一；
INCRBY key increment：将key所存储的值加上给定的增量值；
INCRBYFLOAT key increment：将key所存储的值加上给定的浮点增量值；
DECR key：将key中存储的数字值减一；
DECRBY key decrement：key所存储的值减去给定的减量值；
APPEND key value：key已存在，将value追加到该该key原来值得的末尾；

```

Redis哈希
---------------------------------------------------------------------------------------
```
Redis hash：是一个string类型的field和value的映射表，hash特别适合用于存储对象；
redis中每个hash可以存储2^32-1键值对

hdel key field1[field2]：删除一个或多个哈希表字段；
hexists key field：查看哈希表key中，指定的字段是否存在；
hget key field：获取存储在哈希表中指定字段的值；
hgetall key：获取在哈希表中指定key的所有字段和值；
hincrby key field increment：为哈希表key中的指定字段的整数值加上增量increment；
hincrbyfloat key field increment：为哈希表key中的指定字段的浮点数值加上增量increment；
hkeys key：获取所有哈希表中的字段；
hlen key：获取哈希表中字段的数量；
hmget key field1[field2]：获取所有给定字段的值；
hmget key field1 value1 [field2 value2]：同时将多个field-value对设置到哈希表 key中；
hset key field value：将哈希表key中的字符field的值设为value；
hsetnx key field value：只有在字段field不存在时，设置哈希表字段的值；
hvals key：获取哈希表中的所有值；
hscan key cursor [match pattern][count count]：迭代哈希表中的键值对；

```

Redis列表
----------------------------------------------------------------------------------------
```
Redis：最简单的字符串列表，2^32-1个元素

LPUSH：插入

blpop key1 [key2] timeout：移出并获取列表的第一个元素

```

Redis集合（set)
----------------------------------------------------------------------------------------
```
set是String类型的无序集合。集合成员是唯一的。集合是通过哈希表实现的，增删改查复杂度都是o(1)
最大成员数2^32-1

SADD：插入
```

Redis有序集合（sorted set）
---------------------------------------------------------------------------------------
```
不允许有重复的成员；但分数可以重复；
每个元素都会关联一个double类型的分数，通过分数进行集合成员大小排序。
集合是通过哈希表实现的，增删改查复杂度都是o(1)
最大成员数2^32-1

ZADD：添加

```

Redis HyperLogLog
---------------------------------------------------------------------------------------
```
Redis HyperLogLog：用来做基数统计的算法；
    优点：
        在输入元素的数量或者体积非常大时，计算基数的空间总是固定，并且很小的；
    缺点：
        只会根据输入元素计算基数，不会存储元素本身
```

Redis发布订阅
-------------------------------------------------------------------------------------------
```
Redis发布订阅是一种消息通信模式：发送者pub发送消息，订阅者sub接收消息；
    客户端可以订阅任意数量的频道；
    
```

Redis事务
---------------------------------------------------------------------------------------
```
Redis事务：可以一次执行多个命令，并：
            批量操作在发送EXEC命令前被放入队列缓存；
            收到EXEC命令后进入事务执行，事务中任意命令执行失败，其余命令依然被执行；
            在事务执行过程中，其他客户端提交的命令请求不会插入到事务执行命令序列中；
    一个事务从开始到执行三个阶段：
        开始事务
        命令入列
        执行事务

    MULTI：开始事务
    SET
    GET
    SADD 
    SMEMBERS
    EXEC：触发事务，一并执行命令中的所有事务

    redis事务的执行并不是原子性的；
    一个打包的批量执行脚本；批量指令并非原子化的操作，中间某条指令失败不会对前面已做指令进行回滚，
        也不会造成后续指令不做；

    DISCARD：取消事务，放弃执行事务块内所有命令；
    UNWATCH：取消watch命令对所有key的监视；
    watch key [key...]：监视一个或多个key，如果在事务执行之前这个这些key被其他命令改动，事务将被打断；

```

Redis脚本
------------------------------------------------------------------------------------------------------------
```
Redis：用Lua解释器来执行脚本。

执行Lua脚本：
    Eval script numkeys key [key ...] arg [arg ...]
    EVALSHA sha1 numkeys key [key ...] arg [arg ...]
查看指定脚本是否已保存在缓存中：
    SCRIPT EXISTS script [script ...]
将脚本缓存中移出所有脚本：
    ACRIPT FLUSH
杀死当前正在运行的Lua脚本：
    SCRIPT KILL
将脚本script添加到脚本缓存中，但并不立即执行这个脚本：
    SCRIPT LOAD script

```

Redis连接
----------------------------------------------------------------------------------------
```
Redis连接用于连接Redis服务

AUTH password：验证密码是否正确了；
ECHO message：打印字符串；
PING：查看服务是否运行；
QUIT：关闭当前连接；
SELECT index：切换到指定的数据库；

```

Redis服务器
-------------------------------------------------------------------------------------
```
主要用于管理redis服务；

INFO：

    bgrewriteaof：异步执行一个AOF文件重写操作；
    bgsave：在后台异步保存当前数据库的数据到磁盘；
    client kill [ip:port] [ID client-id]：关闭客户端连接；
    client list：获取连接到服务器的客户端连接列表；
    client getname：获取连接的名称；
    client pause timeout：在指定时间内终止运行来自客户端的命令；
    client setname connection-name：设置当前连接的名称；
    cluster slots：获取集群节点的映射数组；
    command：获取redis命令详情数组；


```

Redis数据备份与恢复
----------------------------------------------------------------------------------
```
SAVE：创建当前数据库的备份；

恢复数据：
    将备份文件移动到redis安装目录并启动服务即可；

获取redis目录：CONFIG  GET dir

Bgsave：也可以备份文件，该命令在后台执行；

```

Redis安全
-----------------------------------------------------------------------------------
```
通过设置密码参数，进行密码验证；

CONFIG get requirepass：查看是否设置了密码验证

```

Redis性能测试
------------------------------------------------------------------------------
```
通过同时执行多个命令实现的；

redis-benchmark [option] [option value]

-h：指定服务器主机名；
-p：指定服务器端口；
-s：指定服务器socket；
-c：指定并发连接数；
-n：指定请求数；
-d：以字节的形式指定set/get值的数据大小；
-k：1=keep alive 0=reconnect；
-r：set/get/incr使用随机key，sadd使用随机值；
-P：通过管道传输<numreq>请求；
-q：强制退出redis，仅显示query/sec的值；
--csv：以csv格式输出；
-i：生成循环，永久执行测试；
-t：仅运行以逗号分隔的测试命令列表；
-I：Idle模式，打开N个idle连接并等待；

```

Redis客户端连接
----------------------------------------------------------------------------------
```
Redis通过监听一个TCP接口、或Unix socket的方式来接收来自客户端的连接。
    一个连接建立后，redis内部会进行以下一些操作：
        1.客户端socket会被设置为非阻塞模式。（Redis在网络事件处理上采用的是非阻塞多路复用模型）
        2.为socket设置TCP_NODELAY属性，禁用Nagle算法；
        3.创建可读的文件事件 用于监听客户端socket的数据发送；
最大连接数：
    maxclients默认是10000

client list：返回连接到redis服务的客户端列表；
client setname：设置当前连接的名称；
client getname：获取服务名称；
client pause：挂起客户端连接，指定挂起的时间毫秒计；
client kill：关闭客户端连接；

```

Redis管道技术
---------------------------------------------------------------------------------------
```
Redis：基于客户端-服务端模型及请求/响应协议的TCP服务；

    客户端向服务端发送一个查询请求，监听Socket返回，以阻塞模式，等待服务端响应；
    服务端处理命令，将结果返回给客户端；

Redis管道技术：
    可以在服务端末响应时，客户端可以继续向服务端发送请求，并最终一次性读取所有服务端的响应；

优势：
    提高了redis服务的性能；

```

Redis分区
-------------------------------------------------------------------------------------
```
分区是分割数据到多个Redis实例的处理过程，因此每个实例只保存key的一个子集；
优势：
    通过利用多台计算机内存的和值，允许我们构造更大的数据库；
    通过多核和多台计算机，允许我们扩展计算能力；
    通过多台计算机和网络适配器，允许我们扩展网络带宽；

不足：
    涉及多个key的操作通常不被支持；
    涉及多个key的redis事务不能使用；
    当使用分区时，数据处理较为复杂，
    增加删除容量也复杂；


分区类型：
    范围分区：映射一定范围的对象到特定的redis实例；
    哈希分区：对任何key都适用
            用一个hash函数转换为一个数字；
            对整个整数取模，将其转化为0-3之间的数字，就可以将这个整数映射到4个redis实例中的一个；
            取模操作是取除的余数 %；

```