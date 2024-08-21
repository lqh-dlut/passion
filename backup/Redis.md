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



