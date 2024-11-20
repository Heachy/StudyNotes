# Redis
- [中文文档](http://www.redis.cn/download.html)
- [官网](https://redis.io/download/)

# Redis 介绍

Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统。它支持多种类型的数据结构，如：

- 字符串（strings）
- 散列（hashes）
- 列表（lists）
- 集合（sets）
- 有序集合（sorted sets）
- 范围查询（range queries）
- bitmaps
- hyperloglogs
- 地理空间（geospatial）索引半径查询

Redis 还提供了以下特性：

- LRU 驱动事件（LRU eviction）
- 事务（transactions）
- Redis 哨兵（Sentinel）系统，用于高可用性
- 自动分区（Cluster）功能，用于扩展性

Redis 提供高可用性（high availability）和可扩展性（scalability）。

你可以对这些类型执行**原子操作**，例如：

- 字符串（strings）的 `SET` 和 `GET` 命令
- 列表（lists）的 `LPUSH` 和 `RPUSH` 命令
- 集合（sets）的 `SADD` 和 `SINTER` 命令，用于计算交集
- 有序集合（sorted sets）的 `ZADD` 和 `ZRANGE` 命令

Redis 的这些特性使得它非常适合用作数据库、缓存和消息代理。

# Redis操作

## 启动redis

`redis-server kconfig/redis.conf`

**使用redis-cli链接测试**

```shell
[root@heachy bin]# redis-cli -p 6379
27.0.0.1:6379> ping
PONG
27.0.0.1:6379> set name helloworld
OK
27.0.0.1:6379> get name
"helloworld"
```
## 切换数据库

> 默认使用的是第0个数据库，可以使用 `SELECT` 命令进行切换数据库。
>

```shell
127.0.0.1:6379> SELECT 3  # 切换到数据库3
OK
127.0.0.1:6379[3]> DBSIZE  # 查看当前数据库大小
(integer) 0
```

## 清除数据库

- **清除当前数据库** 使用 `FLUSHDB` 命令。

```shell
127.0.0.1:6379[3]> FLUSHDB
OK
```

- **清除全部数据库的内容** 使用 `FLUSHALL` 命令。

```shell
127.0.0.1:6379[3]> FLUSHALL
OK
127.0.0.1:6379[3]> keys *
(empty list or set)
```
# Redis 单线程性能解析

## Redis 是单线程的！

明白Redis是很快的，官方表示，Redis是基于内存操作，**CPU不是Redis性能瓶颈**，Redis的瓶颈是根据机器的内存和网络带宽。既然可以使用单线程来实现，就使用单线程了！所有就使用了单线程了！

Redis是C语言写的，官方提供的数据为 **100000+的QPS**，完全不比同样是使用key-value的Memcache差！

## Redis 为什么单线程还这么快？

1. **误区1**：高性能的服务器一定是多线程的？
2. **误区2**：多线程（CPU上下文会切换！）一定比单线程效率高！

先去了解 **CPU > 内存 > 硬盘** 的速度要有所了解！

> **核心**：Redis是将所有的数据全部放在内存中的，所以说使用单线程去操作效率就是最高的，多线程（CPU上下文会切换：耗时的操作！！！），对于内存系统来说，如果没有上下文切换效率就是最高的！多次读写都是在一个CPU上的，在内存情况下，这个就是最佳的方案！

# Redis 基本命令操作

## 设置和获取值

```shell
127.0.0.1:6379> set key1 v1          # 设置值
OK
127.0.0.1:6379> get key1             # 获取值
"v1"
```

## 查看所有键

```shell
127.0.0.1:6379> keys *              # 获取所有的key
1) "key1"
```

## 检查键是否存在

```shell
127.0.0.1:6379> EXISTS key1         # 判断某一个key是否存在
(integer) 1
```

## 追加字符串

```shell
127.0.0.1:6379> APPEND key1 "hello"  # 追加字符串，如果当前key不存在，就相当于setkey
(integer) 7
127.0.0.1:6379> get key1
"v1hello"
```

## 获取字符串长度

```shell
127.0.0.1:6379> STRLEN key1         # 获取字符串的长度
(integer) 7
```

## 步长自增和自减

```shell
127.0.0.1:6379> set views 0         # 初始浏览量为0
OK
127.0.0.1:6379> get views
"0"
127.0.0.1:6379> incr views          # 自增1，浏览量变为1
(integer) 1
127.0.0.1:6379> get views
"1"
127.0.0.1:6379> decr views          # 自减1，浏览量-1
(integer) 0
127.0.0.1:6379> get views
"0"
127.0.0.1:6379> INCRBY views 10     # 可以设置步长，指定增量！
(integer) 10
127.0.0.1:6379> DECRBY views 5      # 可以设置步长，指定减量！
(integer) 5
```

## 字符串范围获取

```shell
127.0.0.1:6379> set key1 "hello,kuangshen"  # 设置 key1 的值
OK
127.0.0.1:6379> get key1
"hello,kuangshen"
127.0.0.1:6379> GETRANGE key1 0 3           # 截取字符串 [0,3]
"hell"
127.0.0.1:6379> GETRANGE key1 0 -1          # 获取全部的字符串 和 get key是一样的
"hello,kuangshen"
```

## 替换字符串

```shell
127.0.0.1:6379> set key2 abcdefg
OK
127.0.0.1:6379> get key2
"abcdefg"
127.0.0.1:6379> SETRANGE key2 1 xx         # 替换指定位置开始的字符串！
(integer) 7
127.0.0.1:6379> get key2
"axxdfg"
```

## 设置过期时间

```shell
127.0.0.1:6379> setex key3 30 "hello"      # 设置key3 的值为 hello,30秒后过期
OK
127.0.0.1:6379> ttl key3                  # 查看剩余时间
(integer) 26
127.0.0.1:6379> get key3
"hello"
127.0.0.1:6379> setnx mykey "redis"       # 如果mykey 不存在，创建mykey
(integer) 1
127.0.0.1:6379> keys *
1) "key2"
2) "mykey"
3) "key1"
127.0.0.1:6379> ttl key3
(integer) -2
127.0.0.1:6379> setnx mykey "MongoDB"     # 如果mykey存在，创建失败！
(integer) 0
127.0.0.1:6379> get mykey
"redis"
```

## 同时设置多个值

```shell
127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3    # 同时设置多个值
OK
127.0.0.1:6379> keys *
1) "k1"
2) "k2"
3) "k3"
127.0.0.1:6379> mget k1 k2 k3            # 同时获取多个值
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> msetnx k1 y1 k4 v4       # msetnx 是一个原子性的操作，要么一起成功，要么一起失败！
(integer) 0
127.0.0.1:6379> get k4
(nil)
```

## 对象存储

```shell
127.0.0.1:6379> set user:1 {"name":"zhangsan","age":3}  # 设置一个user:1 对象 值为 json字符串来保存一个对象！
OK
127.0.0.1:6379> mset user:1:name zhangsan user:1:age 2
OK
127.0.0.1:6379> mget user:1:name user:1:age
1) "zhangsan"
2) "2"
```

## 获取和设置键值（getset）

```shell
127.0.0.1:6379> getset db redis       # 如果不存在值，则返回 nil
(nil)
127.0.0.1:6379> get db
"redis"
127.0.0.1:6379> getset db mongodb    # 如果存在值，获取原来的值，并设置新的值
"redis"
127.0.0.1:6379> get db
"mongodb"
```
# Redis List 操作

在Redis里面，我们可以把list玩成，栈、队列、阻塞队列！

所有的list命令都是用开头的。

## 基本操作

```shell
127.0.0.1:6379> LPUSH list one  # 将一个值或者多个值，插入到列表头部（左）
(integer) 1
127.0.0.1:6379> LPUSH list two
(integer) 2
127.0.0.1:6379> LPUSH list three
(integer) 3
127.0.0.1:6379> LRANGE list 0 -1  # 获取list中值！
1) "three"
2) "two"
3) "one"
127.0.0.1:6379> LRANGE list 0 1  # 通过区间获取具体的值！
1) "three"
2) "two"
127.0.0.1:6379> Rpush list righr  # 将一个值或者多个值，插入到列表尾部（右）
(integer) 4
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "two"
3) "one"
4) "righr"
```

## 移除元素

```shell
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "two"
3) "one"
4) "righr"
127.0.0.1:6379> LPOP list  # 移除list的第一个元素
"three"
127.0.0.1:6379> RPOP list  # 移除list的最后一个元素
"righr"
127.0.0.1:6379> LRANGE list 0 -1
1) "two"
2) "one"
```

## 通过下标获取元素

```shell
127.0.0.1:6379> LRANGE list 0 -1
1) "two"
2) "one"
127.0.0.1:6379> LINDEX list 1  # 通过下标获取list中的某一个值！
"one"
127.0.0.1:6379> LINDEX list 0
"two"
```

## 获取列表长度

```shell
127.0.0.1:6379> LPUSH list one
(integer) 1
127.0.0.1:6379> LPUSH list two
(integer) 2
127.0.0.1:6379> LPUSH list three
(integer) 3
127.0.0.1:6379> LLEN list  # 返回列表的长度
(integer) 3
```

## 移除指定的值

```shell
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "three"
3) "two"
4) "one"
127.0.0.1:6379> LREM list 1 one  # 移除list集合中指定个数的value，精确匹配
(integer) 1
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "three"
3) "two"
127.0.0.1:6379> LREM list 1 three  # 移除list集合中指定个数的value
(integer) 1
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "two"
```

## 修剪列表

```shell
127.0.0.1:6379> keys *
(empty list or set)
127.0.0.1:6379> Rpush mylist "hello"
(integer) 1
127.0.0.1:6379> Rpush mylist "hello1"
(integer) 2
127.0.0.1:6379> Rpush mylist "hello2"
(integer) 3
127.0.0.1:6379> Rpush mylist "hello3"
(integer) 4
127.0.0.1:6379> LTRIM mylist 1 2  # 通过下标截取指定的长度
OK
127.0.0.1:6379> LRANGE mylist 0 -1
1) "hello1"
2) "hello2"
```

## 列表间元素移动

```shell
127.0.0.1:6379> Rpush mylist "hello"
(integer) 1
127.0.0.1:6379> Rpush mylist "hello1"
(integer) 2
127.0.0.1:6379> Rpush mylist "hello2"
(integer) 3
127.0.0.1:6379> RPOPLPUSH mylist myotherlist
"hello2"
127.0.0.1:6379> LRANGE mylist 0 -1
1) "hello"
2) "hello1"
127.0.0.1:6379> LRANGE myotherlist 0 -1
1) "hello2"
```

## 更新操作

```shell
127.0.0.1:6379> EXISTS list  # 判断这个列表是否存在
(integer) 0
127.0.0.1:6379> LSET list 0 item  # 如果不存在列表我们去更新就会报错
(error) ERR no such key
127.0.0.1:6379> LPUSH list value1
(integer) 1
127.0.0.1:6379> LRANGE list 0 0
1) "value1"
127.0.0.1:6379> LSET list 0 item  # 如果存在，更新当前下标的值
OK
127.0.0.1:6379> LRANGE list 0 0
1) "item"
127.0.0.1:6379> LSET list 1 other  # 如果不存在，则会报错！
(error) ERR index out of range
```

## 插入操作

```shell
127.0.0.1:6379> Rpush mylist "hello"
(integer) 1
127.0.0.1:6379> Rpush mylist "world"
(integer) 2
127.0.0.1:6379> LINSERT mylist before "world" "other"  # 将某个具体的value插入到
```

# Redis Set 操作

### 向 Set 中添加元素

```shell
127.0.0.1:6379> sadd myset "hello"
(integer) 1
127.0.0.1:6379> sadd myset "kuangshen"
(integer) 1
127.0.0.1:6379> sadd myset "lovekuangshen"
(integer) 1
127.0.0.1:6379> SMEMBERS myset  # 查看指定set的所有值
1) "hello"
2) "lovekuangshen"
3) "kuangshen"
```

### 判断元素是否在 Set 中

```shell
127.0.0.1:6379> SISMEMBER myset hello  # 判断某一个值是不是在set集合中！
(integer) 1
127.0.0.1:6379> SISMEMBER myset world
(integer) 0
```

### 获取 Set 中的元素个数

```shell
127.0.0.1:6379> SCARD myset  # 获取set集合中的内容元素个数！
(integer) 4
```

### 移除 Set 集合中的指定元素

```shell
127.0.0.1:6379> SREM myset hello  # 移除set集合中的指定元素
(integer) 1
127.0.0.1:6379> SCARD myset
(integer) 3
127.0.0.1:6379> SMEMBERS myset
1) "lovekuangshen2"
2) "lovekuangshen"
3) "kuangshen"
```

### 随机移除 Set 集合中的元素

```shell
127.0.0.1:6379> SPOP myset  # 随机删除一些set集合中的元素！
"lovekuangshen2"
127.0.0.1:6379> SPOP myset
"lovekuangshen"
127.0.0.1:6379> SMEMBERS myset
1) "kuangshen"
```

## 将指定值移动到另一个 Set 集合

```shell
127.0.0.1:6379> sadd myset "hello"
(integer) 1
127.0.0.1:6379> sadd myset "world"
(integer) 1
127.0.0.1:6379> sadd myset "kuangshen"
(integer) 1
127.0.0.1:6379> sadd myset2 "set2"
(integer) 1
127.0.0.1:6379> MOVE myset myset2 "kuangshen"  # 将一个指定的值，移动到另外一个set集合！
(integer) 1
127.0.0.1:6379> SMEMBERS myset
1) "world"
2) "hello"
127.0.0.1:6379> SMEMBERS myset2
1) "kuangshen"
2) "set2"
```

## 数字集合类操作

- 差集 SDIFF
- 交集 SINTER
- 并集 SUNION

```shell
127.0.0.1:6379> SDIFF key1 key2  # 差集
1) "b"
2) "a"
127.0.0.1:6379> SINTER key1 key2  # 交集 共同好友就可以这样实现
1) "c"
127.0.0.1:6379> SUNION key1 key2  # 并集
1) "b"
2) "c"
3) "e"
4) "a"
5) "d"
```
# Redis Hash (哈希) 操作

## Map集合，key-map! 时候这个值是一个map集合！本质和String类型没有太大区别，还是一个简单的key-value！

### 基本操作

```shell
127.0.0.1:6379> set myhash field kuangshen
OK

127.0.0.1:6379> hset myhash field1 kuangshen  # set一个具体 key-value
(integer) 1
127.0.0.1:6379> hget myhash field1  # 获取一个字段值
"kuangshen"

127.0.0.1:6379> hset myhash field1 hello field2 world  # set多个 key-value
OK
127.0.0.1:6379> hmget myhash field1 field2  # 获取多个字段值
1) "hello"
2) "world"

127.0.0.1:6379> hgetall myhash  # 获取全部的数据
1) "field2"
2) "world"
3) "field1"
4) "hello"
```

### 删除指定key字段

```shell
127.0.0.1:6379> hdel myhash field1  # 删除hash指定key字段！对应的value值也就消失了！
(integer) 1
127.0.0.1:6379> hgetall myhash
1) "field2"
2) "world"
```

### 获取hash表的字段数量

```shell
127.0.0.1:6379> hmset myhash field1 hello field2 world
OK
127.0.0.1:6379> HGETALL myhash
1) "field2"
2) "world"
3) "field1"
4) "hello"
127.0.0.1:6379> HLEN myhash  # 获取hash表的字段数量！
(integer) 2
```

### 判断hash中指定字段是否存在

```shell
127.0.0.1:6379> HEXISTS myhash field1  # 判断hash中指定字段是否存在！
(integer) 1
127.0.1:6379> HEXISTS myhash field3
(integer) 0
```

### 获取所有field或value

```shell
127.0.0.1:6379> hkeys myhash  # 只获得所有field
1) "field2"
2) "field1"

127.0.0.1:6379> hvals myhash  # 只获得所有value
1) "world"
2) "hello"
```

### 自增自减操作

```shell
127.0.0.1:6379> hset myhash field3 5  # 指定增量！
(integer) 1
127.0.0.1:6379> HINCRBY myhash field3 1  # 自增
(integer) 6
127.0.0.1:6379> HINCRBY myhash field3 -1  # 自减
(integer) 5

127.0.0.1:6379> hsetnx myhash field4 hello  # 如果不存在则可以设置
(integer) 1
127.0.0.1:6379> hsetnx myhash field4 world  # 如果存在则不能设置
(integer) 0
```

# Zset（有序集合）

在set的基础上，增加了一个值，`set k1 v1` 变为 `zset k1 score1 v1`

```shell
127.0.0.1:6379> zadd myset 1 one      # 添加一个值
(integer) 1
127.0.0.1:6379> zadd myset 2 two 3 three  # 添加多个值
(integer) 2
127.0.0.1:6379> ZRANGE myset 0 -1
1) "one"
2) "two"
3) "three"
```

## 排序如何实现

```shell
127.0.0.1:6379> zadd salary 2500 xiaohong  # 添加三个用户
(integer) 1
127.0.0.1:6379> zadd salary 5000 zhangsan
(integer) 1
127.0.0.1:6379> zadd salary 500 kaungshen
(integer) 1
127.0.0.1:6379> ZRANGEBYSCORE salary -inf +inf  # 显示全部的用户 从小到大！
1) "kaungshen"
2) "xiaohong"
3) "zhangsan"
```

```shell
127.0.0.1:6379> ZRANGEBYSCORE salary -inf +inf withscores  # 显示全部的用户并且附带成绩
1) "kaungshen"
2) "500"
3) "xiaohong"
4) "2500"
5) "zhangsan"
6) "5000"
127.0.0.1:6379> ZRANGEBYSCORE salary -inf 2500 withscores  # 显示工资小于2500员工的升序排序！
1) "kaungshen"
2) "500"
3) "xiaohong"
4) "2500"
```

## 移除rem中的元素

```shell
127.0.0.1:6379> zrange salary 0 -1
1) "kaungshen"
2) "xiaohong"
3) "zhangsan"
127.0.0.1:6379> zrem salary xiaohong
(integer) 1
127.0.0.1:6379> zrange salary 0 -1
1) "kaungshen"
2) "zhangsan"
```

## 其他操作

```shell
127.0.0.1:6379> zrange salary 0 -1
1) "kaungshen"
2) "zhangsan"
127.0.0.1:6379> zcard salary  # 获取有序集合中的个数
(integer) 2
127.0.0.1:6379> zadd myset 1 hello
(integer) 1
127.0.0.1:6379> zadd myset 2 world 3 kaungshen
(integer) 2
127.0.0.1:6379> zcount myset 1 3  # 获取指定区间的成员数量！
(integer) 3
127.0.0.1:6379> zcount myset 1 2
(integer) 2

```
# 地理空间索引操作

## 添加地理位置 (geoadd)

### 规则
两级无法直接添加，通常会下载城市数据，通过java程序一次性导入！

```shell
127.0.0.1:6379> geoadd china:city 39.90 116.40 beijing
(error) ERR invalid longitude,latitude pair 39.900000,116.400000
```

### 参数 key 值

```shell
127.0.0.1:6379> geoadd china:city 116.40 39.90 beijing
(integer) 1
127.0.0.1:6379> geoadd china:city 121.47 31.23 shanghai
(integer) 1
127.0.0.1:6379> geoadd china:city 106.50 29.53 chongqi 114.05 22.52 shenzhen
(integer) 2
127.0.0.1:6379> geoadd china:city 120.16 30.24 hangzhou 108.96 34.26 xian
(integer) 2
```

## 获取指定城市的经纬度 (getpos)

```shell
127.0.0.1:6379> GEOPOS china:city beijing  # 获取指定的城市的经度和纬度！
1) "116.39999896287918091"
2) "39.900000095367431641"
127.0.0.1:6379> GEOPOS china:city beijing chongqi
1) "116.39999896287918091"
2) "39.900000095367431641"
1) "106.499997675418853760"
2) "29.529999577998657227"
```

## 两人之间的距离 (GEODIST)

单位：

- `m` 表示单位为米。
- `km` 表示单位为千米。
- `mi` 表示单位为英里。
- `ft` 表示单位为英尺。

```shell
127.0.0.1:6379> GEODIST china:city beijing shanghai km  # 查看上海到北京的直线距离
"1067.3788"
127.0.0.1:6379> GEODIST china:city beijing chongqi km  # 查看重庆到北京的直线距离
"1464.0708"
```

## 以给定的经纬度为中心，找出某一半径内的元素 (georadius)

```shell
127.0.0.1:6379> GEORADIUS china:city 110 30 1000 km  # 以110.30 这个经纬度为中心，寻找方圆1000km内的城市
1) "chongqi"
2) "xian"
3) "shengzhen"
4) "hangzhou"
```

### 显示到中间距离的位置 (withdist)

```shell
127.0.0.1:6379> GEORADIUS china:city 110 30 500 km withdist  # 显示到中间距离的位置
1) "chongqi"
2) "341.9374"
1) "xian"
2) "483.8340"
```

### 显示他人的定位信息 (withcoord)

```shell
127.0.0.1:6379> GEORADIUS china:city 110 30 500 km withcoord  # 显示他人的定位信息
1) "chongqi"
2) "106.499997675418853760"
3) "29.529999577998657227"
1) "xian"
2) "108.9600017666668167114"
3) "34.259999644189299777"
```

### 筛选出指定的结果 (count)

```shell
127.0.0.1:6379> GEORADIUS china:city 110 30 500 km withdist withcoord count 1  # 筛选出指定的结果！
1) "chongqi"
2) "341.9374"
3) "106.499997675418853760"
4) "29.529999577998657227"
1) "xian"
2) "483.8340"
3) "108.9600017666668167114"
4) "34.259999644189299777"
```

## 找出位于指定元素周围的其他元素 (GEORADIUSBYMEMBER)

```shell
127.0.0.1:6379> GEORADIUSBYMEMBER china:city beijing 1000 km
1) "beijing"
2) "xian"
127.0.0.1:6379> GEORADIUSBYMEMBER china:city shanghai 400 km
1) "hangzhou"
2) "shanghai"
```

## 返回一个或多个位置元素的 Geohash 表示 (GEOHASH)

```shell
127.0.0.1:6379> geohash china:city beijing chongqi
1) "wx4fbxxfke0"
2) "wm5xzrybty0"
```

## 使用 Zset 命令操作 GEO

> GEO 底层的实现原理其实就是 Zset！我们可以使用 Zset 命令来操作 geo！

```shell
127.0.0.1:6379> ZRANGE china:city 0 -1  # 查看地图中全部的元素
1) "chongqi"
2) "xian"
3) "shengzhen"
4) "hangzhou"
5) "shanghai"
6) "beijing"
127.0.0.1:6379> zrem china:city beijing  # 移除指定元素！
(integer) 1
127.0.0.1:6379> ZRANGE china:city 0 -1
1) "chongqi"
2) "xian"
3) "shengzhen"
4) "hangzhou"
5) "shanghai"
```
# Hyperloglog 特殊数据集

## 测试使用

```shell
127.0.0.1:6379> PFadd mykey a b c d e f g h i j  # 创建第一组元素 mykey
(integer) 1
127.0.0.1:6379> PFCOUNT mykey  # 统计 mykey 元素的基数数量
(integer) 10
127.0.0.1:6379> PFadd mykey2 i j z x c v b n m  # 创建第二组元素 mykey2
(integer) 1
127.0.0.1:6379> PFCOUNT mykey2
(integer) 9
127.0.0.1:6379> PFMERGE mykey3 mykey mykey2  # 合并两组 mykey mykey2 => mykey3 并集
OK
127.0.0.1:6379> PFCOUNT mykey3  # 看并集的数量！
(integer) 15
```
# 使用 bitmap 记录打卡

使用 bitmap 来记录周一到周日的打卡情况：
- 周一：1
- 周二：0
- 周三：0
- 周四：1
- ...

## 设置打卡记录

```shell
127.0.0.1:6379> setbit sign 0 1
(integer) 0
127.0.0.1:6379> setbit sign 1 0
(integer) 0
127.0.0.1:6379> setbit sign 2 0
(integer) 0
127.0.0.1:6379> setbit sign 3 1
(integer) 0
127.0.0.1:6379> setbit sign 4 1
(integer) 0
127.0.0.1:6379> setbit sign 5 0
(integer) 0
127.0.0.1:6379> setbit sign 6 0
(integer) 0
```

## 查看某一天是否有打卡

```shell
127.0.0.1:6379> getbit sign 3
(integer) 1
127.0.0.1:6379> getbit sign 6
(integer) 0
```

## 统计打卡的天数

```shell
127.0.0.1:6379> bitcount sign  # 统计这周的打卡记录，就可以看到是否有全勤！
(integer) 3
```
# 事务

Redis 事务本质：一组命令的集合！一个事务中的所有命令都会被序列化，在事务执行过程中，会按照顺序执行！一次性、顺序性、排他性！执行一些列的命令！

## 队列 set set set 执行

Redis 事务没有没有隔离级别的概念！
所有的命令在事务中，并没有直接被执行！只有发起执行命令的时候才会执行！Exec
Redis 单条命令式保存原子性的，但是事务不保证原子性！

redis 的事务：
- 开启事务 (multi)
- 命令入队
- 执行事务 (exec)

## 正常执行事务

```shell
127.0.0.1:6379> multi  # 开启事务
OK
# 命令入队
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> get k2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> exec  # 执行事务
1) OK
2) OK
3) "v2"
4) OK
```

## 放弃事务

```shell
127.0.0.1:6379> multi  # 开启事务
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> set k4 v4
QUEUED
127.0.0.1:6379> DISCARD  # 取消事务
OK
127.0.0.1:6379> get k4  # 事务队列中命令都不会被执行！
(nil)
```

## 编译型异常

> 编译型异常（代码有问题！命令有错！），事务中所有的命令都不会被执行！

```shell
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> getset k3  # 错误的命令
(error) ERR wrong number of arguments for 'getset' command
127.0.0.1:6379> set k4 v4
QUEUED
127.0.0.1:6379> set k5 v5
QUEUED
127.0.0.1:6379> exec  # 执行事务报错！
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> get k5  # 所有的命令都不会被执行！
(nil)
```

## 运行时异常

> 运行时异常（1/0），如果事务队列中存在语法性，那么执行命令的时候，其他命令是可以正常执行的，错误命令抛出异常！

```shell
127.0.0.1:6379> set k1 "v1"
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> incr k1  # 会执行的时候失败！
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> get k3
QUEUED
127.0.0.1:6379> exec
1) (error) ERR value is not an integer or out of range  # 虽然第一条命令报错了，但是依旧正常执行成功了！
2) OK
3) OK
4) "v3"
127.0.0.1:6379> get k2
"v2"
127.0.0.1:6379> get k3
"v3"
```

# Redis 监视测试

## 正常执行成功

```shell
127.0.0.1:6379> set money 100
OK
127.0.0.1:6379> set out 0
OK
127.0.0.1:6379> watch money  # 监视 money 对象
OK
127.0.0.1:6379> multi  # 事务正常结束，数据期间没有发生变动，这个时候就正常执行成功！
OK
127.0.0.1:6379> DECRBY money 20
QUEUED
127.0.0.1:6379> INCRBY out 20
QUEUED
127.0.0.1:6379> exec
1) (integer) 80
2) (integer) 20
```

## 测试多线程修改值，使用 `watch` 可以当做 Redis 的乐观锁操作

```shell
127.0.0.1:6379> watch money  # 监视 money
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> DECRBY money 10
QUEUED
127.0.0.1:6379> INCRBY out 10
QUEUED
127.0.0.1:6379> exec  # 执行之前，另外一个线程，修改了我们的值，这个时候，就会导致事务执行失败！
(nil)
```

## 如果修改失败，获取最新的值就好

```shell
127.0.0.1:6379> UNWATCH  # 如果发现事务执行失败，就先解锁
OK
127.0.0.1:6379> WATCH money  # 获取最新的值，再次监视，select version
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> DECRBY money 1
QUEUED
127.0.0.1:6379> INCRBY money 1
QUEUED
127.0.0.1:6379> exec  # 比对监视的值是否发生了变化，如果没有变化，那么可以执行成功，如果变量就执行失败！
1) (integer) 999
2) (integer) 1000
```

# Redis Java 操作示例

## 初始化 Jedis 连接

```java
public static void main(String[] args) {
    Jedis jedis = new Jedis(host: "127.0.0.1", port: 6379);

    jedis.flushDB();

    JSONObject jsonObject = new JSONObject();
    jsonObject.put("hello", "world");
    jsonObject.put("name", "kuangshen");
    // 开启事务
    Transaction multi = jedis.multi();
    String result = jsonObject.toJSONString();
    // jedis.watch(result);
    try {
        multi.set("user1", result);
        multi.set("user2", result);
        int i = 1/0; // 代码抛出异常事务，执行失败！
        multi.exec(); // 执行事务！
    } catch (Exception e) {
        multi.discard(); // 放弃事务
        e.printStackTrace();
    } finally {
        System.out.println(jedis.get("user1"));
        System.out.println(jedis.get("user2"));
        jedis.close(); // 关闭连接
    }
}
```

# Redis 配置与操作

## 1. 导入依赖

```xml
<!-- 操作redis -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

## 2. 配置连接

```properties
# 配置redis
spring.redis.host=127.0.0.1
spring.redis.port=6379
```

## 3. 使用 Spring Boot 操作 Redis

```java
private RedisTemplate redisTemplate;

@Test
void contextLoads() {
    // redisTemplate 操作不同的数据类型，api和我们的指令是一样的
    // opsForValue 操作字符串 类似String
    // opsForList 操作List 类似List
    // opsForSet 操作Set 类似Set
    // opsForHash
    // opsForZSet
    // opsForGeo
    // opsForHyperLogLog

    // 除了基本的操作，我们常用的方法都可以直接通过redisTemplate操作，比如事务，和基本的CRUD
    // 获取redis的连接对象
    // RedisConnection connection = redisTemplate.getConnectionFactory().getConnection();
    // connection.flushDb();
    // connection.flushAll();

    redisTemplate.opsForValue().set("mykey", "关注狂神说公众号");
    System.out.println(redisTemplate.opsForValue().get("mykey"));
}
```

## 4. 配置文件 unit 单位大小写不敏感

```bash
# 1k => 1000 bytes
# 1kb => 1024 bytes
# 1m => 1000000 bytes
# 1mb => 1024*1024 bytes
# 1g => 1000000000 bytes
# 1gb => 1024*1024*1024 bytes

# units are case insensitive so 1GB 1Gb 1gb are all the same.
```

**注意**：配置文件中的 unit 单位对大小写不敏感。
# Redis 配置文件示例

## 网络设置

```bash
bind 127.0.0.1  # 绑定的IP地址
protected-mode yes  # 保护模式
port 6379  # 端口设置
```

## 通用设置 (GENERAL)

```bash
daemonize yes  # 以守护进程的方式运行，默认是no，需要自己开启为yes！
pidfile /var/run/redis_6379.pid  # 如果以守护进程的方式运行，需要指定一个pid文件路径

# 日志设置
# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)
# warning (only very important / critical messages are logged)
loglevel notice
logfile ""  # 日志文件位置，空字符串表示不记录日志到文件
databases 16  # 数据库的数量，默认是16个数据库
always-show-logo yes  # 是否总是显示LOGO
```
# Redis 快照配置

持久化，在规定的时间内，执行了多少次操作，则会持久化到文件 `.rdb.aof`

Redis 是内存数据库，如果没有持久化，那么数据断电及失！

## 持久化策略

```bash
# 如果900s内，如果至少有一个key进行了修改，我们及进行持久化操作
save 900 1
# 如果300s内，如果至少10 key进行了修改，我们及进行持久化操作
save 300 10
# 如果60s内，如果至少10000 key进行了修改，我们及进行持久化操作
save 60 10000
# 我们之后学习持久化，会自己定义这个测试！

```

# Redis 安全配置

## 设置密码

可以在这里设置Redis的密码，默认是没有密码的！

```shell
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> config get requirepass  # 获取Redis的密码
1) "requirepass"
2) ""
127.0.0.1:6379> config set requirepass "123456"  # 设置Redis的密码
OK
127.0.0.1:6379> config get requirepass  # 发现所有的命令都没有权限了
(error) NOAUTH Authentication required.
127.0.0.1:6379> ping
(error) NOAUTH Authentication required.
127.0.0.1:6379> auth 123456  # 使用密码进行登录！
OK
127.0.0.1:6379> config get requirepass
1) "requirepass"
2) "123456"
```

## 限制 CLIENTS

```bash
maxclients 10000  # 设置能连接上redis的最大客户端的数量
maxmemory <bytes>  # redis 配置最大的内存容量
```

## 内存达到上限后的处理策略 (maxmemory-policy)

```bash
1. volatile-lru: 只对设置了过期时间的key进行LRU（默认值）
2. allkeys-lru: 删除lru算法的key
3. volatile-random: 随机删除即将过期key
4. allkeys-random: 随机删除
5. volatile-ttl: 删除即将过期的
6. noeviction: 永不过期，返回错误
```

## APPEND ONLY 模式 AOF 配置

```bash
appendonly no  # 默认是不开启aof模式的，默认是使用rdb方式持久化的，在大部分所有的情况下，rdb完全够用！
appendfilename "appendonly.aof"  # 持久化的文件的名字

# appendfsync always  # 每次修改都会sync，消耗性能
appendfsync everysec  # 每秒执行一次sync，可能会丢失这1s的数据！
# appendfsync no  # 不执行sync，这个时候操作系统自己同步数据，速度最快！
```

具体的配置，我们在Redis持久化中去给大家详细详解！

## 触发机制

1. save的规则满足的情况下，会自动触发rdb规则
2. 执行flushall命令，也会触发我们的rdb规则！
3. 退出redis，也会产生rdb文件！

## 如何恢复rdb文件

1. 只需要将rdb文件放在我们redis启动目录就可以，redis启动的时候会自动检查dump.rdb 恢复其中的数据！
2. 查看需要存在的位置

```shell
127.0.0.0.1:6379> config get dir
1) "dir"
2) "/usr/local/bin"  # 如果在这个目录下存在 dump.rdb 文件，启动就会自动恢复其中的数据
```