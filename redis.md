 # Redis简介
   redis是完全开源免费的，遵守BSD协议，是一个高性能（NOSQL）的key-value数据库.   
   redis是一个使用C语言编写的，支持网络，可基于内存亦可持久化的日志型，key-value数据库。    
   NoSQL：泛指非关系型数的数据库。主要是解决大规模数据集合多重数据种类带来的挑战。
      数据与数据之间没有关系。   
   SQL:关系型数据库:表与表与之间建立了关联关系。
# NoSQL数据库的四大分类
1. key-value存储数据库 如 redis
2. 列存储数据库 这部分数据库通常是用来应对分布式存储的海量数据。  
键仍然存在，但是他们特定是指向了多个列。这些列是由列家族来安排的。入Cassandra,HBase,Riak.
3. 文档型数据库 灵感来自于办公软件，而且它同第一种键值存储相类似。  
该类型的数据模型是版本化的文档。半结构化的文档以特定的格式存储。  
比如JSON.文档型数据库比键值数据库的查询效率更高。如MongoDb.
4. 图形数据库  

## 特点
原子性：redis的所有操作都是原子性的。  
数据类型：支持String，List，Hash，Set及Orderd Set数据类型操作。  
Redis是一个简单的，高效的，分布式的，基于内存的缓存工具。   
架设好服务器后，通过网络连接（类似数据库），提供key-value式缓存服务。  
简单，是Redis的突出特点。  
简单可以保证核心功能的稳定和优异。  
缺点：耗内存，占用内存过高。

## redis会什么是高效的
1. 开发语言 redis使用c语言开发，c语言是非常贴近操作系统的语言。
2. 纯内存访问 redis将所有数据放在内存中，0次IO。
3. 单线程  
   单线程简化算法的实现。   
   单线程避免了线程的切换和加锁释放锁带来的消耗。   
   非阻塞多路IO复用机制  


## redis.conf配置文件详解
```
   1. redis默认不是以守护进程的方式运行，可以通过配置项修改，使用yes启动守护进程   
      daemonize no // yes
   2. save <seconds> <changes>  持久化的条件。
```

## linux 命令
````
:wq 保存提出
:wq! 强制保存退出
 ps -ef | grep -i redis 查看当前指定进程

 redis-cli 客户端

 redis-server 服务端
 redis-cli -a 123456 带密码登录
  关闭方式：
       1. 查询 PID  ps -ef | grep -i redis 查看当前指定进程
          kill -9 pid        容易造成数据丢失
       2. redis-cli shutdown 正常关系 数据保存
````

## Redis命令描述
```
  1.  redis健key   Redis键命令用于管理redis的键
      1.1 DEL key 删除键
      1.2 dump key 序列化指定key 保存在硬盘
      1.3 exists key 指定key是否存在
      1.4 ttl key 返回指定key的存活时间
      1.5 key * 返回所有 '?'代表一个任意字符
      1.6 type key 返回指定key的数据类型
``` 
# 第一种数据类型 String
```
1. String 是redis最基本的类型，一个Key对应一个value。
2. String类型是二进制安全的。意思是redis的String可以包含任何数据。比如jpg图片或者序列化的对象。
3. 二进制安全是指，在传输数据时，保证二进制数据的信息安全，也就是不被篡改，破译等，如果被攻击。。能够及时检测出来。
4. 一个键最大可以存储5152MB
在传输过程中，是以二进制的形式去传输的，不需要编码解码的过程。不会出现乱码。
```
### string 命令
```
set key value  设置或覆盖。
get key 获取指定的key值。如果key不存在，返回null。如果key存储的值不是字符串类型，返回一个错误。
1. redis中的key和value是区分大小写的，命令不区分大小写。redis是单线程，不适合存储大量数据。
2. incr key  -- 对应的value自增1.如果没有这个key值，自动为你创建 并复制为1
3. decr key  -- 对应的value减一
```

# 第二种类型： Hash:key -filed-value
>>> Hash: 一个JavaBean，是一个String类型的field和Value的映射表。hash特别适合用于存储对象。
>>> 相当于1个key对应一个map 即 一个hash对应多个key-value
>>> 常用来存储一个对象、json. 
>>> Redis的Hash实际是内部存储的value为一个HashMap,并提供了存取这个Map成员的接口。
```
 1. 存储语法 ：hset key filed value
  设定多个filed:  hmset key field1 vaule1 field2 value2 field3 value3..
 2. 取值语法： hget key filed
   取多个指定的field： hget key filed1 filed2
   获取所有filed：hgetall key 
```
![hash存储](https://github.com/zhangyuewen12/JAVA--study/blob/master/pic_folder/QQ%E6%88%AA%E5%9B%BE20190824113245.png?raw=true)

```
hset key filed value 设置值
hget key filed 获取值
```
![hash例子](https://github.com/zhangyuewen12/JAVA--study/blob/master/pic_folder/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20190824113737.png?raw=true)

# 第三种类型:List
```
List 有顺序可重复

　　  lpush list 1  2  3  4 从左添加元素　

    　rpush list 1 2 3 4    从右添加元素

    　lrange list 0 -1 (从0 到-1 元素查看：也就表示查看所有)

    　lpop list （从左边取，删除）

    　rpop list  (从右边取，删除)
```

# 第四种类型：Set
```
Set 无顺序，不能重复　　　

　　  sadd set1 a b c d d (向set1中添加元素) 元素不重复

  　  smembers set1 （查询元素）

  　  srem set1 a （删除元素）
```
# 第五种类型： SortedSet(zset)
```
   有顺序，不能重复

　　适合做排行榜 排序需要一个分数属性

　　zadd zset1 9 a 8 c 10 d 1 e   （添加元素 zadd key score member ）

　　(ZRANGE key start stop [WITHSCORES])(查看所有元素：zrange key  0  -1  withscores)

　　如果要查看分数，加上withscores.

　　zrange zset1 0 -1 (从小到大)

　　zrevrange zset1 0 -1 (从大到小)

　　zincrby zset2 score member (对元素member 增加 score)
```

# Redis的发布订阅
```
1. subscribe channel 订阅给定的一个或多个频道的信息。
2. publish channel message 将消息发送给指定的频道channel
```

# Redis 多数据库
```
Redis下，数据库是由一个整数索引来标识，而不是一个数据名称。默认情况下，一个客户端连接到数据库0；
redis 配置文件中下面的参数来控制数据库总数：
    database 16  // 从0开始 
select 数据库 // 数据库的切换
```
# Redis 事务
>>> mysql中只有使用了InnoDB 引擎的数据库或表才支持事务。
>>> 使用“事务”的目的是：统一管理insert,update,delete这些写操作，以此来维护数据的完整性。
>>> Redis事务可以一次执行多个命令，按顺序地串行化执行，执行中不会被其他命令插入，不许加塞。
```
Redis事务可以一次执行多个命令，并且带有以下三个重要的保证：
1. 批量操作在发送EXEC命令前被放入队列缓存。
2. 收到EXEC命令后进入事务执行，事务中任意命令执行失败，其余的命令依然被执行。
3. 在事务执行过程，其他客户端提交的命令请求不会插入到事务执行命令序列。

单个Redis命令的执行是原子性的，但Redis没有在事务上增加维持原子性的机制。
所以Redis事务的执行并不是原子性的。事务可以理解为一个打包的批量执行脚本。
但批量指令并非原子化的操作，中间某条指令的失败不会导致前面已做指令的回滚，也不会造成后续的指令不做。
这是官网上的说明 From redis docs on transactions:
It's important to note that even when a command fails, all the other commands in the queue are processed – Redis will not stop the processing of commands.
```
## mysql:
```
begin	     #显式地开启一个事务
commit	  #提交事务，对数据库进行的所有写操作变为永久性的
rollback   #结束用户的事务，并撤销正在进行的所有未提交的写操作
```
## mysql的两种引擎Innodb和myisam的区别
```
   MYISAM存储引擎采用的是表级锁，强调的是性能，查询速度比InnoDB类型更快，但是不提供事务支持，InnoDB提供事务支持事务。
   InnoDB存储引擎既支持行级锁，也支持表级锁，默认采用行级锁。

   MyISAM不支持外键，InnoDB支持外键。
1、MyISAM：默认表类型，它是基于传统的ISAM类型，ISAM是Indexed Sequential Access Method (有索引的顺序访问方法) 的缩写，它是存储记录和文件的标准方法。不是事务安全的，而且不支持外键，如果执行大量的select，insert MyISAM比较适合。

2、InnoDB：支持事务安全的引擎，支持外键、行锁、事务是他的最大特点。如果有大量的update和insert，建议使用InnoDB，特别是针对多个并发和QPS较高的情况。   
```


## redis支持简单的事务：
```
multi		#标记事务的开始 输入Multi命令开始，输入的命令都会进入命令队列中，但不会执行
exec		#执行事务的commands队列，Redis会将之前的命令队列中的命令一次执行。
discard		#结束事务，并清除commands队列
```
通过类比msyql来理解redis的事务命令，很显然，它们有着本质区别。
redis和mysql事务的对比（不能回滚）：

```
		  mysql		   				     redis
开启	  start transaction/begin	     multi		
语句	  SQL						     命令	
失败	  roolback回滚				     discart取消
成功	  commit提交					 exec执行事务的命令队列	
```
```
roolback和discard的区别：   
如果已经成功执行了2条语句，第3条语句出错。roolback后前面2条语句的影响小时；
discard 取消事务，放弃执行事务块内的所有命令
```

## 事务的错误处理
```
事务的错误处理：
1. 如果执行的某个命令报出了错误，则只有报错的命令不会被执行，而其他的命令都会执行，不会回滚。
2. 队列中的某个命令出现了报告错误（输入了错误的命令），执行时整个的所有队列都会被取消。
```
## 应用场景
```
一组命令必须同时执行，或者都不执行。
我们想要保住一组命令在执行的过程之中不被其他命令插入。
```

## redis 提供6种数据淘汰策略
```
Redis官方给的警告，当内存不足时，Redis会根据配置的缓存策略淘汰部分keys，以保证写入成功。
当无淘汰策略或没有淘汰的key时，redis直接返回out of memory错误。
```
```
# volatile-lru -> Evict using approximated LRU among the keys with an expire set.
# allkeys-lru -> Evict any key using approximated LRU.
# volatile-lfu -> Evict using approximated LFU among the keys with an expire set.
# allkeys-lfu -> Evict any key using approximated LFU.
# volatile-random -> Remove a random key among the ones with an expire set.
# allkeys-random -> Remove a random key, any key.
# volatile-ttl -> Remove the key with the nearest expire time (minor TTL)
# noeviction -> Don't evict anything, just return an error on write operations.

```

# Redis 持久化
```
数据存放于：
内存：高效，断电后，内存数据会丢失
硬盘：读写速度慢于内存，断电数据不会丢失。
```

## RDB
```
bin dump.rdb redis.conf
RDB：是redis的默认持久化机制。 RDB相当于快照。保存的是一种状态；
几十G的数据可以用快照保存为几十KB；
快照的默认的持久化方式。这种方式就是讲内存中数据以快照的方式写入到二进制文件中。
默认的文件名为dump.rdb
优点：快照保存数据极快，还原数据极快。
适用于灾难备份
快照条件：
  1. 服务器正常关闭时， ./bin/redis-cli shutdown
  2. keys满足一定条件，会进行快照
```

## AOF
```
由于快照方式是在一定间隔时间做一次的，所以如果redis意外down掉的话，就会丢失最后一次快照后的修改。
如果应用要求不能丢失任何修改的话，可以采用AOF持久化方式。
Append-only file:AOF 比快照方式有更好的持久化性能。是由于在使用aof持久化方式时，redis
会将每一个收到的写命令都通过write函数追加到文件中。（默认是appendonly.aof）。
当redis重启时会通过执行文件中保存的写命令来在内存中重建整个数据库的内容。

有三种方式如下（默认是：每秒fsync一次）
 1. appendonly yes // 启用aof持久化方式
 2. appendfsync always // 收到写命令就立即写入磁盘，最慢，但是保证完全的持久化。
 3. appendfsync everysec // 每秒钟写入磁盘一次，在性能和持久化方面做了很好的折中。
 4. appendfsync no//完全依赖os，性能最好，持久化没保证。

 AOF产生的问题：
   aof的方式也同时带来另一个问题。持久化文件会变得越来越大。例如我们调用incr test命令100次。
   文件必须保存全部的100条命令。其中99条都是多余的。
```