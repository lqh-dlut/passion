## Redis
remote dictionary server 基于内存的数据存储系统（数据库，缓存，消息队列）
NoSQL
>  磁盘IO导致的MySQL数据库性能瓶颈

使用方式：
1. 命令行CLI
2. 代码API
3. 图形化GUI -> RedisInsight

windows下运行Redis
<D:\app\redis>redis-server.exe   // 服务器端
<D:\app\redis>redis-cli.exe         // 客户端
**redis-cli.exe --raw**                  // 显示原始数据（否则中文以二进制显示）

### STRING
SET key value
**SETEX key time value  //设置带过期时间的键
SETNX key value   //只有不存在时候才设置成功**
GET key
DEL key
EXISTS key
**KEYS *                  //查找全部键**
KEYS key
KEYS *st               //查找全部以st结尾的键
FLUSHALL            //全部数据删除
**EXPIRE key time  //倒计时**
TTL key               //查看过期时间



### List
LPUSH list key1 key2 key3  //添加到list头部，一次性可添加多个，注意这样加的顺序是key3在头部
RPUSH list key //尾部

> 列表从左到右 0,1,2,3... L可以理解为left，R是right

LRANGE list 0 -1  //输出列表元素，0 -1表示从头到尾
LPOP/RPOP list count //从头/尾删除count个元素
LTRIM list start end  //删除范围之外的元素
LLEN list //length
RPOPLPUSH //最简单的先进先出的队列

### SET
> 无序集合，元素唯一不可以重复，没有顺序

SADD set key     // 添加
SMEMBERS set  // 输出
SISMEMBER set key //判断是否是集合中元素
SREM set key //删除
SINTER set1 set2 ... //输出集合的交集
SUNION // 并集

### SortedSet (ZSet)
> 有序集合，每一个唯一的元素关联一个不唯一的可重复的分数，按分数**从小到大**排序

ZADD zset score1 key1 score2 key2 
ZREM zset key
ZRANGE zset start end [WITHSCORES]  //（带分数的）输出 
ZCARD zset // 成员数量
ZSCORE zset key // 输出元素对应的分数
ZRANK zset key // 输出元素分数的排名（从小到大的排名）
ZREVRANK // 从大到小的排名
ZCOUNT zset min max // 范围内的成员数量


### HASH

HSET hash key val
HGET hash key
HGETALL hash // 成对出现的哈希对
HDEL hash key
HEXISTS hash key


### STREAM
XADD stream *  field message // * 表示随机消息id，id可自定义[time-number] 返回的是消息id的值
XLEN stream 
XRANGE stream - +  // - +代表的是从头到尾，可数字范围
XDEL stream id
XREAD COUNT num BLOCK millisecond STREAMS stream start   //num代表返回的条目数量，BLOCK是阻塞多少毫秒，start是开始的序列号
XREAD COUNT 3 BLOCK 10000  STREAMS streamtest $   // **用' $ '表示从现在开始接收到的新消息**

> consumer group 消费者组

XGROUP CREAT stream group id MKSTREAM  // 创建消费者组 id表示该消费者组从何处开始读取信息，可省略使用SETID命令单独设置，而MKSTREAM表示若不存在则新创建一个stream
XGROUP CREATE/DELCONSUMER stream group consumer  // 加入/删除消费者
XREADGROUP GROUP group consumer COUNT num BLOCK time STREAMS stream id  // 为consumer读取消息，id为0表示从一开始开始读取，**id为' > '表示读取最新的消息**

### GEO 地理位置

GEOADD key longitude latitude member ...   // 添加member经纬信息
GEOPOS key member  // 显示位置信息
GEODIST key mem1 mem2 [KM]  [WITHDIST] [WITHCOORD] // 计算量地址的直线距离，默认是米为单位，添加KM转换
GEORESEARCH key FROMMEMBER mem1 BYRADIUS/ BYBOX num [KM] // 圆形矩形范围内的成员 withdist,withcoord表示附上距离，坐标

### HyperLoglog 估算基数

> 基数：不重复数，原理使用随机算法计算，精度没那么高但性能好，适合**大量的不需要高精度的数据**，e.g.浏览量

PFADD cardinal key1 key2 ...     // 添加基数 
PFCOUNT cardinal
PFMERGE result cardinal1 cardinal2  //合并到result中

### Bitmap 位图
> 字符串类型的扩展，0和1表示一个bit数组

SETBIT bitmap index val
SET bitmap "11000101" / "\xC5"   //单独和完整设置bitmap的值，可利用二进制或" \x "转为十六进制方便
BITCOUNT bitmap start end  //返回bitmap中1的个数
BITPOS bitmap 0/1 start end  // 出现的第一个0/1的位置 

### Bitfield 位域
BITFIELD fieldid set [u8/i8/...] #index key
BITFIELD fieldid get [format] #index
BITFIELD fieldid INCRBY [u8/i8/...] #index key

### 事务
MULTI ... EXEC 
> 使用队列缓存事务中的命令，如果有命令出现错误，不影响其他正确的命令，其他用户的命令不会插入队列中

> 通过conf配置文件实现主从复制

### sentinel 哨兵结点

> 监控，通知，故障转移

哨兵结点本身也是单节点，一般使用三个哨兵结点实现高可用，通过选举的方式选择新的主节点
哨兵之间通信使用的是发布/订阅功能

 