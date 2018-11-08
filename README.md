Redis
===============================================================
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
```