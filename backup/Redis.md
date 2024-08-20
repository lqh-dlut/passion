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




