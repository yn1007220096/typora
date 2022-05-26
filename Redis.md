

## 一、Nosql概述

### 为什么使用Nosql

> 1、单机Mysql时代

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020082010365930.png#pic_center)

90年代,一个网站的访问量一般不会太大，单个数据库完全够用。随着用户增多，网站出现以下问题

1. 数据量增加到一定程度，单机数据库就放不下了
2. 数据的索引（B+ Tree）,一个机器内存也存放不下
3. 访问量变大后（读写混合），一台服务器承受不住。

> 2、Memcached(缓存) + Mysql + 垂直拆分（读写分离）

网站80%的情况都是在读，每次都要去查询数据库的话就十分的麻烦！所以说我们希望减轻数据库的压力，我们可以使用缓存来保证效率！

##### ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200820103713734.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

优化过程经历了以下几个过程：

1. 优化数据库的数据结构和索引(难度大)
2. 文件缓存，通过IO流获取比每次都访问数据库效率略高，但是流量爆炸式增长时候，IO流也承受不了
3. MemCache,当时最热门的技术，通过在数据库和数据库访问层之间加上一层缓存，第一次访问时查询数据库，将结果保存到缓存，后续的查询先检查缓存，若有直接拿去使用，效率显著提升。

> 3、分库分表 + 水平拆分 + Mysql集群

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200820103739584.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

> 4、如今最近的年代

 如今信息量井喷式增长，各种各样的数据出现（用户定位数据，图片数据等），大数据的背景下关系型数据库（RDBMS）无法满足大量数据要求。Nosql数据库就能轻松解决这些问题。

> 目前一个基本的互联网项目

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200820103804572.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

> 为什么要用NoSQL ？

用户的个人信息，社交网络，地理位置。用户自己产生的数据，用户日志等等爆发式增长！
这时候我们就需要使用NoSQL数据库的，Nosql可以很好的处理以上的情况！

### 什么是Nosql

**NoSQL = Not Only SQL（不仅仅是SQL）**

Not Only Structured Query Language

关系型数据库：列+行，同一个表下数据的结构是一样的。

非关系型数据库：数据存储没有固定的格式，并且可以进行横向扩展。

NoSQL泛指非关系型数据库，随着web2.0互联网的诞生，传统的关系型数据库很难对付web2.0时代！尤其是超大规模的高并发的社区，暴露出来很多难以克服的问题，NoSQL在当今大数据环境下发展的十分迅速，Redis是发展最快的。

### Nosql特点

1. 方便扩展（数据之间没有关系，很好扩展！）

2. 大数据量高性能（Redis一秒可以写8万次，读11万次，NoSQL的缓存记录级，是一种细粒度的缓存，性能会比较高！）

3. 数据类型是多样型的！（不需要事先设计数据库，随取随用）

4. 传统的 RDBMS 和 NoSQL

   ```
   传统的 RDBMS(关系型数据库)
   - 结构化组织
   - SQL
   - 数据和关系都存在单独的表中 row col
   - 操作，数据定义语言
   - 严格的一致性
   - 基础的事务
   - ...
   ```

   ```
   Nosql
   - 不仅仅是数据
   - 没有固定的查询语言
   - 键值对存储，列存储，文档存储，图形数据库（社交关系）
   - 最终一致性
   - CAP定理和BASE
   - 高性能，高可用，高扩展
   - ...
   ```

> 了解：3V + 3高

大数据时代的3V ：主要是**描述问题**的

1. 海量Velume
2. 多样Variety
3. 实时Velocity

大数据时代的3高 ： 主要是**对程序的要求**

1. 高并发
2. 高可扩
3. 高性能

真正在公司中的实践：NoSQL + RDBMS 一起使用才是最强的。

### 阿里巴巴演进分析

推荐阅读：阿里云的这群疯子https://yq.aliyun.com/articles/653511

![1](https://img-blog.csdnimg.cn/20200820103829446.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200820103851613.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

```bash
# 商品信息
- 一般存放在关系型数据库：Mysql,阿里巴巴使用的Mysql都是经过内部改动的。

# 商品描述、评论(文字居多)
- 文档型数据库：MongoDB

# 图片
- 分布式文件系统 FastDFS
- 淘宝：TFS
- Google: GFS
- Hadoop: HDFS
- 阿里云: oss

# 商品关键字 用于搜索
- 搜索引擎：solr,elasticsearch
- 阿里：Isearch 多隆

# 商品热门的波段信息
- 内存数据库：Redis，Memcache

# 商品交易，外部支付接口
- 第三方应用
```

### Nosql的四大分类

> **KV键值对**

- 新浪：**Redis**
- 美团：Redis + Tair
- 阿里、百度：Redis + Memcache

> **文档型数据库（bson数据格式）：**

- **MongoDB**(掌握)
  - 基于分布式文件存储的数据库。C++编写，用于处理大量文档。
  - MongoDB是RDBMS和NoSQL的中间产品。MongoDB是非关系型数据库中功能最丰富的，NoSQL中最像关系型数据库的数据库。
- ConthDB

> **列存储数据库**

- **HBase**(大数据必学)
- 分布式文件系统

> **图关系数据库**

用于广告推荐，社交网络

- **Neo4j**、InfoGrid

| **分类**                | **Examples举例**                                   | **典型应用场景**                                             | **数据模型**                                    | **优点**                                                     | **缺点**                                                     |
| ----------------------- | -------------------------------------------------- | ------------------------------------------------------------ | ----------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **键值对（key-value）** | Tokyo Cabinet/Tyrant, Redis, Voldemort, Oracle BDB | 内容缓存，主要用于处理大量数据的高访问负载，也用于一些日志系统等等。 | Key 指向 Value 的键值对，通常用hash table来实现 | 查找速度快                                                   | 数据无结构化，通常只被当作字符串或者二进制数据               |
| **列存储数据库**        | Cassandra, HBase, Riak                             | 分布式的文件系统                                             | 以列簇式存储，将同一列数据存在一起              | 查找速度快，可扩展性强，更容易进行分布式扩展                 | 功能相对局限                                                 |
| **文档型数据库**        | CouchDB, MongoDb                                   | Web应用（与Key-Value类似，Value是结构化的，不同的是数据库能够了解Value的内容） | Key-Value对应的键值对，Value为结构化数据        | 数据结构要求不严格，表结构可变，不需要像关系型数据库一样需要预先定义表结构 | 查询性能不高，而且缺乏统一的查询语法。                       |
| **图形(Graph)数据库**   | Neo4J, InfoGrid, Infinite Graph                    | 社交网络，推荐系统等。专注于构建关系图谱                     | 图结构                                          | 利用图结构相关算法。比如最短路径寻址，N度关系查找等          | 很多时候需要对整个图做计算才能得出需要的信息，而且这种结构不太好做分布式的集群 |

## 二、Redis入门

### 概述

> Redis是什么？

Redis（Remote Dictionary Server )，即远程字典服务。

是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、**Key-Value数据库**，并提供多种语言的API。

与memcached一样，为了保证效率，**数据都是缓存在内存中**。区别的是redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了master-slave(主从)同步。

> Redis能该干什么？

1. 内存存储、持久化，内存是断电即失的，所以需要持久化（RDB、AOF）
2. 高效率、用于高速缓冲
3. 发布订阅系统
4. 地图信息分析
5. 计时器、计数器(eg：浏览量)
6. 。。。

> 特性

1. 多样的数据类型

2. 持久化

3. 集群

4. 事务

   …

### 环境搭建

官网：https://redis.io/

推荐使用Linux服务器学习。

windows版本的Redis已经停更很久了…

### Windows安装

https://github.com/dmajkic/redis

1. 解压安装包
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200820103922318.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)
2. 开启redis-server.exe
3. 启动redis-cli.exe测试![在这里插入图片描述](https://img-blog.csdnimg.cn/20200820103950934.png#pic_center)

### Linux安装



1. 下载安装包：https://redis.io/

2. 解压Redis的安装包！

   ```bash
   tar xvf 文件名
   ```

   程序一般放在 `/opt` 目录下

   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200820104016426.png#pic_center)

3. 基本环境安装

   ```bash
   yum install gcc-c++
   # 然后进入redis目录下执行
   make
   # 然后执行
   make install
   ```

​     make可能出错

```bash
server.c:5346:31: 错误：‘struct redisServer’没有名为‘server_cpulist’的成员
     redisSetCpuAffinity(server.server_cpulist);
                               ^
server.c: 在函数‘hasActiveChildProcess’中:
server.c:1478:1: 警告：在有返回值的函数中，控制流程到达函数尾 [-Wreturn-type]
 }
 ^
server.c: 在函数‘allPersistenceDisabled’中:
server.c:1484:1: 警告：在有返回值的函数中，控制流程到达函数尾 [-Wreturn-type]
 }
 ^
server.c: 在函数‘writeCommandsDeniedByDiskError’中:
server.c:3934:1: 警告：在有返回值的函数中，控制流程到达函数尾 [-Wreturn-type]
 }
 ^
server.c: 在函数‘iAmMaster’中:
server.c:5134:1: 警告：在有返回值的函数中，控制流程到达函数尾 [-Wreturn-type]
 }
 ^
make[1]: *** [server.o] 错误 1
```

原因是gcc版本过低  升级即可

```bash
yum -y install centos-release-scl
 
yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils
 
scl enable devtoolset-9 bash
 
echo "source /opt/rh/devtoolset-9/enable" >> /etc/profile
```



![cd](https://img-blog.csdnimg.cn/20200820104048327.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

1. redis默认安装路径  cd `/usr/local/bin`![在这里插入图片描述](https://img-blog.csdnimg.cn/20200820104140692.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

2. 将redis的配置文件复制到 程序安装目录 `/usr/local/bin/kconfig`下   原生的配置文件还在解压目录下 保证安全

   ```bash
   mkdir kconf
   ```

   ![[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-hxvGQ47d-1597890996509)(狂神说 Redis.assets/image-20200813114000868.png)]](https://img-blog.csdnimg.cn/20200820104157817.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

3. redis默认不是后台启动的，需要修改配置文件！daemonizae 改为yes    

   ```bash
   vim redis.config
   #shift+方向键翻页 i:切换输入模式 esc退出  :wq 保存出
   ```

   ![[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-dDdKTUgd-1597890996510)(狂神说 Redis.assets/image-20200813114019063.png)]](https://img-blog.csdnimg.cn/20200820104213706.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

4. 通过制定的配置文件启动redis服务  有可能没提示

   ![[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-jOypL57Z-1597890996511)(狂神说 Redis.assets/image-20200813114030597.png)]](https://img-blog.csdnimg.cn/20200820104228556.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

5. 使用redis-cli连接指定的端口号测试，Redis的默认端口6379   ping命令可用于检测redis实例是否存活，如果存活则显示PONG   

   keys * 可以看有哪些keyps

   ![[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-LnDaISQ4-1597890996512)(狂神说 Redis.assets/image-20200813114045299.png)]](https://img-blog.csdnimg.cn/20200820104243223.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

6. 查看redis进程是否开启    ctrl+shift+t 打开另一个终端

   ![[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-9PhN1jC1-1597890996513)(狂神说 Redis.assets/image-20200813114103769.png)]](https://img-blog.csdnimg.cn/20200820104300532.png#pic_center)

7. 关闭Redis服务 `shutdown`

   ![[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-Y54EuOYm-1597890996514)(狂神说 Redis.assets/image-20200813114116691.png)]](https://img-blog.csdnimg.cn/20200820104314297.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

8. 再次查看进程是否存在  

   ```bash
   ps -ef|grep redis
   ```

9. 后面我们会使用单机多Redis启动集群测试

### Mac安装

1、没有安装Homebrew，首先安装npm国内的吧，快一些。

```
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```


2、使用Homebrew安装命令

```
brew install redis
```

4、启动redis服务

```
//方式一：使用brew帮助我们启动软件
brew services start redis
//方式二
redis-server /usr/local/etc/redis.conf
```




```
redis-server
```

5、查看redis服务进程

查看redis是否正在运行

```
ps axu | grep redis
```


6、新开终端 redis-cli连接redis服务

redis默认端口号6379，默认auth为空，输入以下命令即可连接

```
redis-cli -h 127.0.0.1 -p 6379
```



7、启动 redis 客户端，打开终端并输入命令 redis-cli。该命令会连接本地的 redis 服务。

```
$redis-cli
redis 127.0.0.1:6379>
redis 127.0.0.1:6379> PING
PONG
```

在以上实例中我们连接到本地的 redis 服务并执行 PING 命令，该命令用于检测 redis 服务是否启动。

8、关闭redis服务

正确停止Redis的方式应该是向Redis发送SHUTDOWN命令

```
redis-cli shutdown
```


强行终止redis

sudo pkill redis-server
1
9、redis.conf 配置文件详解

redis默认是前台启动，如果我们想以守护进程的方式运行（后台运行），可以在redis.conf中将daemonize no,修改成yes即可。
————————————————
版权声明：本文为CSDN博主「Metashop」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/realize_dream/article/details/106227622

### 测试性能

redis-benchmark 是压力测试工具

**redis-benchmark：**Redis官方提供的性能测试工具，参数选项如下：

![img](https://img-blog.csdnimg.cn/20200513214125892.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

**简单测试：**

```bash
# 测试：100个并发连接 100000请求
redis-benchmark -h localhost -p 6379 -c 100 -n 100000
12
```

```bash
redis-server kconfig/redis.conf
redis -cli -p 6379  # 测试服务端是否开启
ping # 测试服务端是否开启

```



![[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-plMshjFg-1597890996515)(狂神说 Redis.assets/image-20200813114143365.png)]](https://img-blog.csdnimg.cn/20200820104343472.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

### 基础知识

> redis默认有16个数据库

![[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-v2S3n3Si-1597890996516)(狂神说 Redis.assets/image-20200813114158322.png)]](https://img-blog.csdnimg.cn/20200820104357466.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

默认使用的第0个;

16个数据库为：DB 0~DB 15
默认使用DB 0 ，可以使用`select n`切换到DB n

![image-20210108152456410](C:\Users\yunan10\AppData\Roaming\Typora\typora-user-images\image-20210108152456410.png)

`dbsize`可以查看当前数据库的大小，与key数量相关。

![image-20210108152824527](C:\Users\yunan10\AppData\Roaming\Typora\typora-user-images\image-20210108152824527.png)

```bash
127.0.0.1:6379> config get databases # 命令行查看数据库数量databases
1) "databases"
2) "16"

127.0.0.1:6379> select 8 # 切换数据库 DB 8
OK
127.0.0.1:6379[8]> dbsize # 查看数据库大小
(integer) 0

# 不同数据库之间 数据是不能互通的，并且dbsize 是根据库中key的个数。
127.0.0.1:6379> set name sakura 
OK
127.0.0.1:6379> SELECT 8
OK
127.0.0.1:6379[8]> get name # db8中并不能获取db0中的键值对。
(nil)
127.0.0.1:6379[8]> DBSIZE
(integer) 0
127.0.0.1:6379[8]> SELECT 0
OK
127.0.0.1:6379> keys *
1) "counter:__rand_int__"
2) "mylist"
3) "name"
4) "key:__rand_int__"
5) "myset:__rand_int__"
127.0.0.1:6379> DBSIZE # size和key个数相关
(integer) 5
```

`keys *` ：查看当前数据库中所有的key。

`flushdb`：清空当前数据库中的键值对。

`flushall`：清空所有数据库的键值对。

> **Redis是单线程的，Redis是基于内存操作的。**

所以Redis的性能瓶颈不是CPU,而是机器内存和网络带宽。

那么为什么Redis的速度如此快呢，性能这么高呢？redis 是C语言写的 QPS达到10W+，完全不比同样使用key-value的Memecache差

> **Redis为什么单线程还这么快？** 

- 误区1：高性能的服务器一定是多线程的？
- 误区2：多线程（CPU上下文会切换！）一定比单线程效率高！

核心：Redis是将所有的数据放在内存中的，所以说使用单线程去操作效率就是最高的，多线程（CPU上下文会切换：耗时的操作！），对于内存系统来说，如果没有上下文切换效率就是最高的，多次读写都是在一个CPU上的，在内存存储数据情况下，单线程就是最佳的方案。

  6.0以后是多线程了：引入了worker Thread，只处理网络IO读取和写入，核心IO负责串行处理客户端指令

## 三、五大数据类型

 Redis是一个开源（BSD许可），内存存储的数据结构服务器，可用作**数据库**，**高速缓存**和**消息队列代理**。它支持[字符串](https://www.redis.net.cn/tutorial/3508.html)、[哈希表](https://www.redis.net.cn/tutorial/3509.html)、[列表](https://www.redis.net.cn/tutorial/3510.html)、[集合](https://www.redis.net.cn/tutorial/3511.html)、[有序集合](https://www.redis.net.cn/tutorial/3512.html)，[位图](https://www.redis.net.cn/tutorial/3508.html)，[hyperloglogs](https://www.redis.net.cn/tutorial/3513.html)等数据类型。内置复制、[Lua脚本](https://www.redis.net.cn/tutorial/3516.html)、LRU收回、[事务](https://www.redis.net.cn/tutorial/3515.html)以及不同级别磁盘持久化功能，同时通过Redis Sentinel提供高可用，通过Redis Cluster提供自动[分区](https://www.redis.net.cn/tutorial/3524.html)。

### Redis-key

> 在redis中无论什么数据类型，在数据库中都是以key-value形式保存，通过进行对Redis-key的操作，来完成对数据库中数据的操作。

下面学习的命令：

- `exists key`：判断键是否存在
- `del key`：删除键值对
- `move key db`：将键值对移动到指定数据库
- `expire key second`：设置键值对的过期时间
- `type key`：查看value的数据类型

```bash
127.0.0.1:6379> keys * # 查看当前数据库所有key
(empty list or set)
127.0.0.1:6379> set name qinjiang # set key
OK
127.0.0.1:6379> set age 20
OK
127.0.0.1:6379> keys *
1) "age"
2) "name"
127.0.0.1:6379> move age 1 # 将键值对移动到指定数据库
(integer) 1
127.0.0.1:6379> EXISTS age # 判断键是否存在
(integer) 0 # 不存在
127.0.0.1:6379> EXISTS name
(integer) 1 # 存在
127.0.0.1:6379> SELECT 1
OK
127.0.0.1:6379[1]> keys *
1) "age"
127.0.0.1:6379[1]> del age # 删除键值对
(integer) 1 # 删除个数


127.0.0.1:6379> set age 20
OK
127.0.0.1:6379> EXPIRE age 15 # 设置键值对的过期时间

(integer) 1 # 设置成功 开始计数
127.0.0.1:6379> ttl age # 查看key的过期剩余时间
(integer) 13
127.0.0.1:6379> ttl age
(integer) 11
127.0.0.1:6379> ttl age
(integer) 9
127.0.0.1:6379> ttl age
(integer) -2 # -2 表示key过期，-1表示key未设置过期时间

127.0.0.1:6379> get age # 过期的key 会被自动delete
(nil)
127.0.0.1:6379> keys *
1) "name"

127.0.0.1:6379> type name # 查看value的数据类型
string
```

关于`TTL`命令

Redis的key，通过TTL命令返回key的过期时间，一般来说有3种：

1. 当前key没有设置过期时间，所以会返回-1.
2. 当前key有设置过期时间，而且key已经过期，所以会返回-2.
3. 当前key有设置过期时间，且key还没有过期，故会返回key的正常剩余时间.

关于重命名`RENAME`和`RENAMENX`

- `RENAME key newkey`修改 key 的名称
- `RENAMENX key newkey`仅当 newkey 不存在时，将 key 改名为 newkey 。

更多命令学习：https://www.redis.net.cn/order/

[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-wBVZtGVm-1597890996517)(狂神说 Redis.assets/image-20200813114228439.png)]

### String(字符串)

| 命令                                 | 描述                                                         | 示例                                                         |
| ------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `APPEND key value`                   | 向指定的key的value后追加字符串                               | 127.0.0.1:6379> set msg hello OK 127.0.0.1:6379> append msg " world" (integer) 11 127.0.0.1:6379> get msg “hello world” |
| `DECR/INCR key`                      | 将指定key的value数值进行+1/-1(仅对于数字)                    | 127.0.0.1:6379> set age 20 OK 127.0.0.1:6379> incr age (integer) 21 127.0.0.1:6379> decr age (integer) 20 |
| `INCRBY/DECRBY key n`                | 按指定的步长对数值进行加减                                   | 127.0.0.1:6379> INCRBY age 5 (integer) 25 127.0.0.1:6379> DECRBY age 10 (integer) 15 |
| `INCRBYFLOAT key n`                  | 为数值加上浮点型数值                                         | 127.0.0.1:6379> INCRBYFLOAT age 5.2 “20.2”                   |
| `STRLEN key`                         | 获取key保存值的字符串长度                                    | 127.0.0.1:6379> get msg “hello world” 127.0.0.1:6379> STRLEN msg (integer) 11 |
| `GETRANGE key start end`             | 按起止位置获取字符串（闭区间，起止位置都取）                 | 127.0.0.1:6379> get msg “hello world” 127.0.0.1:6379> GETRANGE msg 3 9 “lo worl” |
| `SETRANGE key offset value`          | 用指定的value 替换key中 offset开始的值                       | 127.0.0.1:6379> SETRANGE msg 2 hello (integer) 7 127.0.0.1:6379> get msg “tehello” |
| `GETSET key value`                   | 将给定 key 的值设为 value ，并返回 key 的旧值(old value)。   | 127.0.0.1:6379> GETSET msg test “hello world”                |
| `SETNX key value`                    | 仅当key不存在时进行set                                       | 127.0.0.1:6379> SETNX msg test (integer) 0 127.0.0.1:6379> SETNX name sakura (integer) 1 |
| `SETEX key seconds value`            | set 键值对并设置过期时间                                     | 127.0.0.1:6379> setex name 10 root OK 127.0.0.1:6379> get name (nil) |
| `MSET key1 value1 [key2 value2..]`   | 批量set键值对                                                | 127.0.0.1:6379> MSET k1 v1 k2 v2 k3 v3 OK                    |
| `MSETNX key1 value1 [key2 value2..]` | 批量设置键值对，仅当参数中所有的key都不存在时执行            | 127.0.0.1:6379> MSmeETNX k1 v1 k4 v4 (integer) 0             |
| `MGET key1 [key2..]`                 | 批量获取多个key保存的值                                      | 127.0.0.1:6379> MGET k1 k2 k3 1) “v1” 2) “v2” 3) “v3”        |
| `PSETEX key milliseconds value`      | 和 SETEX 命令相似，但它以毫秒为单位设置 key 的生存时间，     |                                                              |
| `getset key value`                   | 如果不存在值，则返回nil，如果存在值，获取原来的值，并设置新的值 |                                                              |

String类似的使用场景：value除了是字符串还可以是数字，用途举例：

- 计数器
- 统计多单位的数量：uid:123666：follow 0
- 粉丝数
- 对象存储缓存

### List(列表)

> Redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）
>
> 一个列表最多可以包含 232 - 1 个元素 (4294967295, 每个列表超过40亿个元素)。

首先我们列表，可以经过规则定义将其变为队列、栈、双端队列等

![[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-VPvbIltc-1597890996518)(狂神说 Redis.assets/image-20200813114255459.png)]](https://img-blog.csdnimg.cn/20200820104440398.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

正如图Redis中List是可以进行双端操作的，所以命令也就分为了LXXX和RLLL两类，有时候L也表示List例如LLEN

| 命令                                    | 描述                                                         |
| --------------------------------------- | ------------------------------------------------------------ |
| `LPUSH/RPUSH key value1[value2..]`      | 从左边/右边向列表中PUSH值(一个或者多个)。                    |
| `LRANGE key start end`                  | 获取list 起止元素==（索引从左往右 递增）==                   |
| `LPUSHX/RPUSHX key value`               | 向已存在的列表中Lpush值（一个或者多个）                      |
| `LINSERT key BEFORE|AFTER value`        | 在指定列表元素的前/后 插入value                              |
| `LLEN key`                              | 查看列表长度                                                 |
| `LINDEX key index`                      | 通过索引获取列表元素                                         |
| `LSET key index value`                  | 通过索引为元素设值                                           |
| `LPOP/RPOP key`                         | 从最左边/最右边移除值 并返回                                 |
| `RPOPLPUSH source destination`          | 将列表的尾部(右)最后一个值弹出，并返回，然后加到另一个列表的头部 |
| `LTRIM key start end`                   | 通过下标截取指定范围内的列表                                 |
| `LREM key count value`                  | List中是允许value重复的 `count > 0`：从头部开始搜索 然后删除指定的value 至多删除count个 `count < 0`：从尾部开始搜索… `count = 0`：删除列表中所有的指定value。 |
| `BLPOP/BRPOP key1[key2] timout`         | 移出并获取列表的第一个/最后一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| `BRPOPLPUSH source destination timeout` | 和`RPOPLPUSH`功能相同，如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |

```bash
---------------------------LPUSH---RPUSH---LRANGE--------------------------------

127.0.0.1:6379> LPUSH mylist k1 # LPUSH mylist=>{1}
(integer) 1
127.0.0.1:6379> LPUSH mylist k2 # LPUSH mylist=>{2,1}
(integer) 2
127.0.0.1:6379> RPUSH mylist k3 # RPUSH mylist=>{2,1,3}
(integer) 3
127.0.0.1:6379> get mylist # 普通的get是无法获取list值的
(error) WRONGTYPE Operation against a key holding the wrong kind of value
127.0.0.1:6379> LRANGE mylist 0 4 # LRANGE 获取起止位置范围内的元素
1) "k2"
2) "k1"
3) "k3"
127.0.0.1:6379> LRANGE mylist 0 2
1) "k2"
2) "k1"
3) "k3"
127.0.0.1:6379> LRANGE mylist 0 1
1) "k2"
2) "k1"
127.0.0.1:6379> LRANGE mylist 0 -1 # 获取全部元素
1) "k2"
2) "k1"
3) "k3"

---------------------------LPUSHX---RPUSHX-----------------------------------

127.0.0.1:6379> LPUSHX list v1 # list不存在 LPUSHX失败
(integer) 0
127.0.0.1:6379> LPUSHX list v1 v2  
(integer) 0
127.0.0.1:6379> LPUSHX mylist k4 k5 # 向mylist中 左边 PUSH k4 k5
(integer) 5
127.0.0.1:6379> LRANGE mylist 0 -1
1) "k5"
2) "k4"
3) "k2"
4) "k1"
5) "k3"

---------------------------LINSERT--LLEN--LINDEX--LSET----------------------------

127.0.0.1:6379> LINSERT mylist after k2 ins_key1 # 在k2元素后 插入ins_key1
(integer) 6
127.0.0.1:6379> LRANGE mylist 0 -1
1) "k5"
2) "k4"
3) "k2"
4) "ins_key1"
5) "k1"
6) "k3"
127.0.0.1:6379> LLEN mylist # 查看mylist的长度
(integer) 6
127.0.0.1:6379> LINDEX mylist 3 # 获取下标为3的元素
"ins_key1"
127.0.0.1:6379> LINDEX mylist 0
"k5"
127.0.0.1:6379> LSET mylist 3 k6 # 将下标3的元素 set值为k6
OK
127.0.0.1:6379> LRANGE mylist 0 -1
1) "k5"
2) "k4"
3) "k2"
4) "k6"
5) "k1"
6) "k3"

---------------------------LPOP--RPOP--------------------------

127.0.0.1:6379> LPOP mylist # 左侧(头部)弹出
"k5"
127.0.0.1:6379> RPOP mylist # 右侧(尾部)弹出
"k3"

---------------------------RPOPLPUSH--------------------------

127.0.0.1:6379> LRANGE mylist 0 -1
1) "k4"
2) "k2"
3) "k6"
4) "k1"
127.0.0.1:6379> RPOPLPUSH mylist newlist # 将mylist的最后一个值(k1)弹出，加入到newlist的头部
"k1"
127.0.0.1:6379> LRANGE newlist 0 -1
1) "k1"
127.0.0.1:6379> LRANGE mylist 0 -1
1) "k4"
2) "k2"
3) "k6"

---------------------------LTRIM--------------------------

127.0.0.1:6379> LTRIM mylist 0 1 # 截取mylist中的 0~1部分
OK
127.0.0.1:6379> LRANGE mylist 0 -1
1) "k4"
2) "k2"

# 初始 mylist: k2,k2,k2,k2,k2,k2,k4,k2,k2,k2,k2
---------------------------LREM--------------------------

127.0.0.1:6379> LREM mylist 3 k2 # 从头部开始搜索 至多删除3个 k2
(integer) 3
# 删除后：mylist: k2,k2,k2,k4,k2,k2,k2,k2

127.0.0.1:6379> LREM mylist -2 k2 #从尾部开始搜索 至多删除2个 k2
(integer) 2
# 删除后：mylist: k2,k2,k2,k4,k2,k2


---------------------------BLPOP--BRPOP--------------------------

mylist: k2,k2,k2,k4,k2,k2
newlist: k1

127.0.0.1:6379> BLPOP newlist mylist 30 # 从newlist中弹出第一个值，mylist作为候选
1) "newlist" # 弹出
2) "k1"
127.0.0.1:6379> BLPOP newlist mylist 30
1) "mylist" # 由于newlist空了 从mylist中弹出
2) "k2"
127.0.0.1:6379> BLPOP newlist 30
(30.10s) # 超时了

127.0.0.1:6379> BLPOP newlist 30 # 我们连接另一个客户端向newlist中push了test, 阻塞被解决。
1) "newlist"
2) "test"
(12.54s)
```

> 小结

- list实际上是一个链表，before Node after , left, right 都可以插入值
- **如果key不存在，则创建新的链表**
- 如果key存在，新增内容
- 如果移除了所有值，空链表，也代表不存在
- 在两边插入或者改动值，效率最高！修改中间元素，效率相对较低

**应用：**

**消息排队！消息队列（Lpush Rpop）,栈（Lpush Lpop）**

### Set(集合)

> Redis的Set是**string类型**的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据。
>
> Redis 中 集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。
>
> 集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储40多亿个成员)。

| 命令                                      | 描述                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| `SADD key member1[member2..]`             | 向集合中无序增加一个/多个成员                                |
| `SCARD key`                               | 获取集合的成员数                                             |
| `SMEMBERS key`                            | 返回集合中所有的成员                                         |
| `SISMEMBER key member`                    | 查询member元素是否是集合的成员,结果是无序的                  |
| `SRANDMEMBER key [count]`                 | 随机返回集合中count个成员，count缺省值为1                    |
| `SPOP key [count]`                        | 随机移除并返回集合中count个成员，count缺省值为1              |
| `SMOVE source destination member`         | 将source集合的成员member移动到destination集合                |
| `SREM key member1[member2..]`             | 移除集合中一个/多个成员                                      |
| `SDIFF key1[key2..]`                      | 返回所有集合的差集 key1- key2 - …                            |
| `SDIFFSTORE destination key1[key2..]`     | 在SDIFF的基础上，将结果保存到集合中==(覆盖)==。不能保存到其他类型key噢！ |
| `SINTER key1 [key2..]`                    | 返回所有集合的交集                                           |
| `SINTERSTORE destination key1[key2..]`    | 在SINTER的基础上，存储结果到集合中。覆盖                     |
| `SUNION key1 [key2..]`                    | 返回所有集合的并集                                           |
| `SUNIONSTORE destination key1 [key2..]`   | 在SUNION的基础上，存储结果到及和张。覆盖                     |
| `SSCAN KEY [MATCH pattern] [COUNT count]` | 在大量数据环境下，使用此命令遍历集合中元素，每次遍历部分     |

```bash
---------------SADD--SCARD--SMEMBERS--SISMEMBER--------------------

127.0.0.1:6379> SADD myset m1 m2 m3 m4 # 向myset中增加成员 m1~m4
(integer) 4
127.0.0.1:6379> SCARD myset # 获取集合的成员数目
(integer) 4
127.0.0.1:6379> smembers myset # 获取集合中所有成员
1) "m4"
2) "m3"
3) "m2"
4) "m1"
127.0.0.1:6379> SISMEMBER myset m5 # 查询m5是否是myset的成员
(integer) 0 # 不是，返回0
127.0.0.1:6379> SISMEMBER myset m2
(integer) 1 # 是，返回1
127.0.0.1:6379> SISMEMBER myset m3
(integer) 1

---------------------SRANDMEMBER--SPOP----------------------------------

127.0.0.1:6379> SRANDMEMBER myset 3 # 随机返回3个成员
1) "m2"
2) "m3"
3) "m4"
127.0.0.1:6379> SRANDMEMBER myset # 随机返回1个成员
"m3"
127.0.0.1:6379> SPOP myset 2 # 随机移除并返回2个成员
1) "m1"
2) "m4"
# 将set还原到{m1,m2,m3,m4}

---------------------SMOVE--SREM----------------------------------------

127.0.0.1:6379> SMOVE myset newset m3 # 将myset中m3成员移动到newset集合
(integer) 1
127.0.0.1:6379> SMEMBERS myset
1) "m4"
2) "m2"
3) "m1"
127.0.0.1:6379> SMEMBERS newset
1) "m3"
127.0.0.1:6379> SREM newset m3 # 从newset中移除m3元素
(integer) 1
127.0.0.1:6379> SMEMBERS newset
(empty list or set)

# 下面开始是多集合操作,多集合操作中若只有一个参数默认和自身进行运算
# setx=>{m1,m2,m4,m6}, sety=>{m2,m5,m6}, setz=>{m1,m3,m6}

-----------------------------SDIFF------------------------------------

127.0.0.1:6379> SDIFF setx sety setz # 等价于setx-sety-setz
1) "m4"
127.0.0.1:6379> SDIFF setx sety # setx - sety
1) "m4"
2) "m1"
127.0.0.1:6379> SDIFF sety setx # sety - setx
1) "m5"


-------------------------SINTER---------------------------------------
# 共同关注（交集）

127.0.0.1:6379> SINTER setx sety setz # 求 setx、sety、setx的交集
1) "m6"
127.0.0.1:6379> SINTER setx sety # 求setx sety的交集
1) "m2"
2) "m6"

-------------------------SUNION---------------------------------------

127.0.0.1:6379> SUNION setx sety setz # setx sety setz的并集
1) "m4"
2) "m6"
3) "m3"
4) "m2"
5) "m1"
6) "m5"
127.0.0.1:6379> SUNION setx sety # setx sety 并集
1) "m4"
2) "m6"
3) "m2"
4) "m1"
5) "m5"
```

### Hash（哈希）

> Redis hash 是一个string类型的field和value的映射表，hash特别适合用于存储对象。
>
> Set就是一种简化的Hash,只变动key,而value使用默认值填充。可以将一个Hash表作为一个对象进行存储，表中存放对象的信息。

| 命令                                             | 描述                                                         |
| ------------------------------------------------ | ------------------------------------------------------------ |
| `HSET key field value`                           | 将哈希表 key 中的字段 field 的值设为 value 。重复设置同一个field会覆盖,返回0 |
| `HMSET key field1 value1 [field2 value2..]`      | 同时将多个 field-value (域-值)对设置到哈希表 key 中。        |
| `HSETNX key field value`                         | 只有在字段 field 不存在时，设置哈希表字段的值。              |
| `HEXISTS key field`                              | 查看哈希表 key 中，指定的字段是否存在。                      |
| `HGET key field value`                           | 获取存储在哈希表中指定字段的值                               |
| `HMGET key field1 [field2..]`                    | 获取所有给定字段的值                                         |
| `HGETALL key`                                    | 获取在哈希表key 的所有字段和值                               |
| `HKEYS key`                                      | 获取哈希表key中所有的字段                                    |
| `HLEN key`                                       | 获取哈希表中字段的数量                                       |
| `HVALS key`                                      | 获取哈希表中所有值                                           |
| `HDEL key field1 [field2..]`                     | 删除哈希表key中一个/多个field字段                            |
| `HINCRBY key field n`                            | 为哈希表 key 中的指定字段的整数值加上增量n，并返回增量后结果 一样只适用于整数型字段 |
| `HINCRBYFLOAT key field n`                       | 为哈希表 key 中的指定字段的浮点数值加上增量 n。              |
| `HSCAN key cursor [MATCH pattern] [COUNT count]` | 迭代哈希表中的键值对。                                       |

```bash
------------------------HSET--HMSET--HSETNX----------------
127.0.0.1:6379> HSET studentx name sakura # 将studentx哈希表作为一个对象，设置name为sakura
(integer) 1
127.0.0.1:6379> HSET studentx name gyc # 重复设置field进行覆盖，并返回0
(integer) 0
127.0.0.1:6379> HSET studentx age 20 # 设置studentx的age为20
(integer) 1
127.0.0.1:6379> HMSET studentx sex 1 tel 15623667886 # 设置sex为1，tel为15623667886
OK
127.0.0.1:6379> HSETNX studentx name gyc # HSETNX 设置已存在的field
(integer) 0 # 失败
127.0.0.1:6379> HSETNX studentx email 12345@qq.com
(integer) 1 # 成功

----------------------HEXISTS--------------------------------
127.0.0.1:6379> HEXISTS studentx name # name字段在studentx中是否存在
(integer) 1 # 存在
127.0.0.1:6379> HEXISTS studentx addr
(integer) 0 # 不存在

-------------------HGET--HMGET--HGETALL-----------
127.0.0.1:6379> HGET studentx name # 获取studentx中name字段的value
"gyc"
127.0.0.1:6379> HMGET studentx name age tel # 获取studentx中name、age、tel字段的value
1) "gyc"
2) "20"
3) "15623667886"
127.0.0.1:6379> HGETALL studentx # 获取studentx中所有的field及其value
 1) "name"
 2) "gyc"
 3) "age"
 4) "20"
 5) "sex"
 6) "1"
 7) "tel"
 8) "15623667886"
 9) "email"
10) "12345@qq.com"


--------------------HKEYS--HLEN--HVALS--------------
127.0.0.1:6379> HKEYS studentx # 查看studentx中所有的field
1) "name"
2) "age"
3) "sex"
4) "tel"
5) "email"
127.0.0.1:6379> HLEN studentx # 查看studentx中的字段数量
(integer) 5
127.0.0.1:6379> HVALS studentx # 查看studentx中所有的value
1) "gyc"
2) "20"
3) "1"
4) "15623667886"
5) "12345@qq.com"

-------------------------HDEL--------------------------
127.0.0.1:6379> HDEL studentx sex tel # 删除studentx 中的sex、tel字段
(integer) 2
127.0.0.1:6379> HKEYS studentx
1) "name"
2) "age"
3) "email"

-------------HINCRBY--HINCRBYFLOAT------------------------
127.0.0.1:6379> HINCRBY studentx age 1 # studentx的age字段数值+1
(integer) 21
127.0.0.1:6379> HINCRBY studentx name 1 # 非整数字型字段不可用
(error) ERR hash value is not an integer
127.0.0.1:6379> HINCRBYFLOAT studentx weight 0.6 # weight字段增加0.6
"90.8"
```

 Hash变更的数据user name age，尤其是用户信息之类的，经常变动的信息！**Hash更适合于对象的存储，Sring更加适合字符串存储！**

### Zset（有序集合）

> 不同的是每个元素都会关联一个double类型的分数（score）。redis正是通过分数来为集合中的成员进行从小到大的排序。
>
> score相同：按字典顺序排序
>
> 有序集合的成员是唯一的,但分数(score)却可以重复。

| 命令                                              | 描述                                                         |
| ------------------------------------------------- | ------------------------------------------------------------ |
| `ZADD key score member1 [score2 member2]`         | 向有序集合添加一个或多个成员，或者更新已存在成员的分数       |
| `ZCARD key`                                       | 获取有序集合的成员数                                         |
| `ZCOUNT key min max`                              | 计算在有序集合中指定区间score的成员数                        |
| `ZINCRBY key n member`                            | 有序集合中对指定成员的分数加上增量 n                         |
| `ZSCORE key member`                               | 返回有序集中，成员的分数值                                   |
| `ZRANK key member`                                | 返回有序集合中指定成员的索引                                 |
| `ZRANGE key start end`                            | 通过索引区间返回有序集合成指定区间内的成员                   |
| `ZRANGEBYLEX key min max`                         | 通过字典区间返回有序集合的成员                               |
| `ZRANGEBYSCORE key min max`                       | 通过分数返回有序集合指定区间内的成员==-inf 和 +inf分别表示最小最大值，只支持开区间()== |
| `ZLEXCOUNT key min max`                           | 在有序集合中计算指定字典区间内成员数量                       |
| `ZREM key member1 [member2..]`                    | 移除有序集合中一个/多个成员                                  |
| `ZREMRANGEBYLEX key min max`                      | 移除有序集合中给定的字典区间的所有成员                       |
| `ZREMRANGEBYRANK key start stop`                  | 移除有序集合中给定的排名区间的所有成员                       |
| `ZREMRANGEBYSCORE key min max`                    | 移除有序集合中给定的分数区间的所有成员                       |
| `ZREVRANGE key start end`                         | 返回有序集中指定区间内的成员，通过索引，分数从高到底         |
| `ZREVRANGEBYSCORRE key max min`                   | 返回有序集中指定分数区间内的成员，分数从高到低排序           |
| `ZREVRANGEBYLEX key max min`                      | 返回有序集中指定字典区间内的成员，按字典顺序倒序             |
| `ZREVRANK key member`                             | 返回有序集合中指定成员的排名，有序集成员按分数值递减(从大到小)排序 |
| `ZINTERSTORE destination numkeys key1 [key2 ..]`  | 计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 key 中，numkeys：表示参与运算的集合数，将score相加作为结果的score |
| `ZUNIONSTORE destination numkeys key1 [key2..]`   | 计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 key 中 |
| `ZSCAN key cursor [MATCH pattern\] [COUNT count]` | 迭代有序集合中的元素（包括元素成员和元素分值）               |

```bash
-------------------ZADD--ZCARD--ZCOUNT--------------
127.0.0.1:6379> ZADD myzset 1 m1 2 m2 3 m3 # 向有序集合myzset中添加成员m1 score=1 以及成员m2 score=2..
(integer) 2
127.0.0.1:6379> ZCARD myzset # 获取有序集合的成员数
(integer) 2
127.0.0.1:6379> ZCOUNT myzset 0 1 # 获取score在 [0,1]区间的成员数量
(integer) 1
127.0.0.1:6379> ZCOUNT myzset 0 2
(integer) 2

----------------ZINCRBY--ZSCORE--------------------------
127.0.0.1:6379> ZINCRBY myzset 5 m2 # 将成员m2的score +5
"7"
127.0.0.1:6379> ZSCORE myzset m1 # 获取成员m1的score
"1"
127.0.0.1:6379> ZSCORE myzset m2
"7"

--------------ZRANK--ZRANGE-----------------------------------
127.0.0.1:6379> ZRANK myzset m1 # 获取成员m1的索引，索引按照score排序，score相同索引值按字典顺序顺序增加
(integer) 0
127.0.0.1:6379> ZRANK myzset m2
(integer) 2
127.0.0.1:6379> ZRANGE myzset 0 1 # 获取索引在 0~1的成员
1) "m1"
2) "m3"
127.0.0.1:6379> ZRANGE myzset 0 -1 # 获取全部成员
1) "m1"
2) "m3"
3) "m2"

#testset=>{abc,add,amaze,apple,back,java,redis} score均为0
------------------ZRANGEBYLEX---------------------------------
127.0.0.1:6379> ZRANGEBYLEX testset - + # 返回所有成员
1) "abc"
2) "add"
3) "amaze"
4) "apple"
5) "back"
6) "java"
7) "redis"
127.0.0.1:6379> ZRANGEBYLEX testset - + LIMIT 0 3 # 分页 按索引显示查询结果的 0,1,2条记录
1) "abc"
2) "add"
3) "amaze"
127.0.0.1:6379> ZRANGEBYLEX testset - + LIMIT 3 3 # 显示 3,4,5条记录
1) "apple"
2) "back"
3) "java"
127.0.0.1:6379> ZRANGEBYLEX testset (- [apple # 显示 (-,apple] 区间内的成员
1) "abc"
2) "add"
3) "amaze"
4) "apple"
127.0.0.1:6379> ZRANGEBYLEX testset [apple [java # 显示 [apple,java]字典区间的成员
1) "apple"
2) "back"
3) "java"

-----------------------ZRANGEBYSCORE---------------------
127.0.0.1:6379> ZRANGEBYSCORE myzset 1 10 # 返回score在 [1,10]之间的的成员
1) "m1"
2) "m3"
3) "m2"
127.0.0.1:6379> ZRANGEBYSCORE myzset 1 5
1) "m1"
2) "m3"

--------------------ZLEXCOUNT-----------------------------
127.0.0.1:6379> ZLEXCOUNT testset - +
(integer) 7
127.0.0.1:6379> ZLEXCOUNT testset [apple [java
(integer) 3

------------------ZREM--ZREMRANGEBYLEX--ZREMRANGBYRANK--ZREMRANGEBYSCORE--------------------------------
127.0.0.1:6379> ZREM testset abc # 移除成员abc
(integer) 1
127.0.0.1:6379> ZREMRANGEBYLEX testset [apple [java # 移除字典区间[apple,java]中的所有成员
(integer) 3
127.0.0.1:6379> ZREMRANGEBYRANK testset 0 1 # 移除排名0~1的所有成员
(integer) 2
127.0.0.1:6379> ZREMRANGEBYSCORE myzset 0 3 # 移除score在 [0,3]的成员
(integer) 2


# testset=> {abc,add,apple,amaze,back,java,redis} score均为0
# myzset=> {(m1,1),(m2,2),(m3,3),(m4,4),(m7,7),(m9,9)}
----------------ZREVRANGE--ZREVRANGEBYSCORE--ZREVRANGEBYLEX-----------
127.0.0.1:6379> ZREVRANGE myzset 0 3 # 按score递减排序，然后按索引，返回结果的 0~3
1) "m9"
2) "m7"
3) "m4"
4) "m3"
127.0.0.1:6379> ZREVRANGE myzset 2 4 # 返回排序结果的 索引的2~4
1) "m4"
2) "m3"
3) "m2"
127.0.0.1:6379> ZREVRANGEBYSCORE myzset 6 2 # 按score递减顺序 返回集合中分数在[2,6]之间的成员
1) "m4"
2) "m3"
3) "m2"
127.0.0.1:6379> ZREVRANGEBYLEX testset [java (add # 按字典倒序 返回集合中(add,java]字典区间的成员
1) "java"
2) "back"
3) "apple"
4) "amaze"

-------------------------ZREVRANK---------------------------
127.0.0.1:6379> ZREVRANK myzset m7 # 按score递减顺序，返回成员m7索引
(integer) 1
127.0.0.1:6379> ZREVRANK myzset m2
(integer) 4


# mathscore=>{(xm,90),(xh,95),(xg,87)} 小明、小红、小刚的数学成绩
# enscore=>{(xm,70),(xh,93),(xg,90)} 小明、小红、小刚的英语成绩
-------------------ZINTERSTORE--ZUNIONSTORE-----------------------------------
127.0.0.1:6379> ZINTERSTORE sumscore 2 mathscore enscore # 将mathscore enscore进行合并 结果存放到sumscore
(integer) 3
127.0.0.1:6379> ZRANGE sumscore 0 -1 withscores # 合并后的score是之前集合中所有score的和
1) "xm"
2) "160"
3) "xg"
4) "177"
5) "xh"
6) "188"

127.0.0.1:6379> ZUNIONSTORE lowestscore 2 mathscore enscore AGGREGATE MIN # 取两个集合的成员score最小值作为结果的
(integer) 3
127.0.0.1:6379> ZRANGE lowestscore 0 -1 withscores
1) "xm"
2) "70"
3) "xg"
4) "87"
5) "xh"
6) "93"
```

应用案例：

- set排序 存储班级成绩表 工资表排序！
- 普通消息，1.重要消息 2.带权重进行判断
- 排行榜应用实现，取Top N测试

## 四、三种特殊数据类型

### Geospatial(地理位置)

> 使用经纬度定位地理坐标并用一个**有序集合zset保存**，所以zset命令也可以使用

| 命令                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `geoadd key longitud(经度) latitude(纬度) member [..]`       | 将具体经纬度的坐标存入一个有序集合                           |
| `geopos key member [member..]`                               | 获取集合中的一个/多个成员坐标                                |
| `geodist key member1 member2 [unit]`                         | 返回两个给定位置之间的距离。默认以米作为单位。               |
| `georadius key longitude latitude radius m|km|mi|ft [WITHCOORD][WITHDIST] [WITHHASH] [COUNT count]` | 以给定的经纬度为中心， 返回集合包含的位置元素当中， 与中心的距离不超过给定最大距离的所有位置元素。 |
| `GEORADIUSBYMEMBER key member radius...`                     | 功能与GEORADIUS相同，只是中心位置不是具体的经纬度，而是使用结合中已有的成员作为中心点。 |
| `geohash key member1 [member2..]`                            | 返回一个或多个位置元素的Geohash表示。使用Geohash位置52点整数编码。 |

**有效经纬度**

> - 有效的经度从-180度到180度。
> - 有效的纬度从-85.05112878度到85.05112878度。

指定单位的参数 **unit** 必须是以下单位的其中一个：

- **m** 表示单位为米。
- **km** 表示单位为千米。
- **mi** 表示单位为英里。
- **ft** 表示单位为英尺。

**关于GEORADIUS的参数**

> 通过`georadius`就可以完成 **附近的人**功能
>
> withcoord:带上坐标
>
> withdist:带上距离，单位与半径单位相同
>
> COUNT n : 只显示前n个(按距离递增排序)

```bash
----------------georadius---------------------
127.0.0.1:6379> GEORADIUS china:city 120 30 500 km withcoord withdist # 查询经纬度(120,30)坐标500km半径内的成员
1) 1) "hangzhou"
   2) "29.4151"
   3) 1) "120.20000249147415"
      2) "30.199999888333501"
2) 1) "shanghai"
   2) "205.3611"
   3) 1) "121.40000134706497"
      2) "31.400000253193539"
     
------------geohash---------------------------
127.0.0.1:6379> geohash china:city yichang shanghai # 获取成员经纬坐标的geohash表示
1) "wmrjwbr5250"
2) "wtw6ds0y300"
```

### Hyperloglog(基数统计)

> Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定的、并且是很小的。
>
> 花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基数。
>
> 因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。
>
> 其底层使用string数据类型

**什么是基数？**

> 数据集中不重复的元素的个数。

**应用场景：**

网页的访问量（UV）：一个用户多次访问，也只能算作一个人。

> 传统实现，存储用户的id,然后每次进行比较。当用户变多之后这种方式及其浪费空间，而我们的目的只是**计数**，Hyperloglog就能帮助我们利用最小的空间完成。

| 命令                                      | 描述                                      |
| ----------------------------------------- | ----------------------------------------- |
| `PFADD key element1 [elememt2..]`         | 添加指定元素到 HyperLogLog 中             |
| `PFCOUNT key [key]`                       | 返回给定 HyperLogLog 的基数估算值。       |
| `PFMERGE destkey sourcekey [sourcekey..]` | 将多个 HyperLogLog 合并为一个 HyperLogLog |

```bash
----------PFADD--PFCOUNT---------------------
127.0.0.1:6379> PFADD myelemx a b c d e f g h i j k # 添加元素
(integer) 1
127.0.0.1:6379> type myelemx # hyperloglog底层使用String
string
127.0.0.1:6379> PFCOUNT myelemx # 估算myelemx的基数
(integer) 11
127.0.0.1:6379> PFADD myelemy i j k z m c b v p q s
(integer) 1
127.0.0.1:6379> PFCOUNT myelemy
(integer) 11

----------------PFMERGE-----------------------
127.0.0.1:6379> PFMERGE myelemz myelemx myelemy # 合并myelemx和myelemy 成为myelemz
OK
127.0.0.1:6379> PFCOUNT myelemz # 估算基数
(integer) 17
```

如果允许容错，那么一定可以使用Hyperloglog !

如果不允许容错，就使用set或者自己的数据类型即可 ！

### BitMaps(位图)

> 使用位存储，信息状态只有 0 和 1
>
> Bitmap是一串连续的2进制数字（0或1），每一位所在的位置为偏移(offset)，在bitmap上可执行AND,OR,XOR,NOT以及其它位操作。

**应用场景**

签到统计、状态统计

| 命令                                  | 描述                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| `setbit key offset value`             | 为指定key的offset位设置值                                    |
| `getbit key offset`                   | 获取offset位的值                                             |
| `bitcount key [start end]`            | 统计字符串被设置为1的bit数，也可以指定统计范围按字节         |
| `bitop operration destkey key[key..]` | 对一个或多个保存二进制位的字符串 key 进行位元操作，并将结果保存到 destkey 上。 |
| `BITPOS key bit [start] [end]`        | 返回字符串里面第一个被设置为1或者0的bit位。start和end只能按字节,不能按位 |

```bash
------------setbit--getbit--------------
127.0.0.1:6379> setbit sign 0 1 # 设置sign的第0位为 1 
(integer) 0
127.0.0.1:6379> setbit sign 2 1 # 设置sign的第2位为 1  不设置默认 是0
(integer) 0
127.0.0.1:6379> setbit sign 3 1
(integer) 0
127.0.0.1:6379> setbit sign 5 1
(integer) 0
127.0.0.1:6379> type sign
string

127.0.0.1:6379> getbit sign 2 # 获取第2位的数值
(integer) 1
127.0.0.1:6379> getbit sign 3
(integer) 1
127.0.0.1:6379> getbit sign 4 # 未设置默认是0
(integer) 0

-----------bitcount----------------------------
127.0.0.1:6379> BITCOUNT sign # 统计sign中为1的位数
(integer) 4
```

**bitmaps的底层**

[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-9PlszjhS-1597890996519)(D:\我\MyBlog\狂神说 Redis.assets\image-20200803234336175.png)]

这样设置以后你能get到的值是：**\xA2\x80**，所以bitmaps是一串从左到右的二进制串

## 五、事务

Redis的单条命令是保证原子性的，但是redis事务不能保证原子性

> Redis事务本质：一组命令的集合。
>
> ----------------- 队列 set set set 执行 -------------------
>
> 事务中每条命令都会被序列化，执行过程中按顺序执行，不允许其他命令进行干扰。
>
> - 一次性
> - 顺序性
> - 排他性
>
> ------
>
> 1. Redis事务没有隔离级别的概念
> 2. Redis单条命令是保证原子性的，但是事务不保证原子性！

### Redis事务操作过程

- 开启事务（`multi`）
- 命令入队
- 执行事务（`exec`）

所以事务中的命令在加入时都没有被执行，直到提交时才会开始执行(Exec)一次性完成。

```bash
127.0.0.1:6379> multi # 开启事务
OK
127.0.0.1:6379> set k1 v1 # 命令入队
QUEUED
127.0.0.1:6379> set k2 v2 # ..
QUEUED
127.0.0.1:6379> get k1
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> keys *
QUEUED
127.0.0.1:6379> exec # 事务执行
1) OK
2) OK
3) "v1"
4) OK
5) 1) "k3"
   2) "k2"
   3) "k1"
```

**取消事务(`discurd`)**

```bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> DISCARD # 放弃事务
OK
127.0.0.1:6379> EXEC 
(error) ERR EXEC without MULTI # 当前未开启事务
127.0.0.1:6379> get k1 # 被放弃事务中命令并未执行
(nil)
```

### 事务错误

> 代码语法错误（编译时异常）所有的命令都不执行

```bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> error k1 # 这是一条语法错误命令
(error) ERR unknown command `error`, with args beginning with: `k1`, # 会报错但是不影响后续命令入队 
127.0.0.1:6379> get k2
QUEUED
127.0.0.1:6379> EXEC
(error) EXECABORT Transaction discarded because of previous errors. # 执行报错
127.0.0.1:6379> get k1 
(nil) # 其他命令并没有被执行
```

> 代码逻辑错误 (运行时异常) **其他命令可以正常执行 ** >>> 所以不保证事务原子性

```bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> INCR k1 # 这条命令逻辑错误（对字符串进行增量）
QUEUED
127.0.0.1:6379> get k2
QUEUED
127.0.0.1:6379> exec
1) OK
2) OK
3) (error) ERR value is not an integer or out of range # 运行时报错
4) "v2" # 其他命令正常执行

# 虽然中间有一条命令报错了，但是后面的指令依旧正常执行成功了。
# 所以说Redis单条指令保证原子性，但是Redis事务不能保证原子性。
```

### 监控

**悲观锁：**

- 很悲观，认为什么时候都会出现问题，无论做什么都会加锁

**乐观锁：**

- 很乐观，认为什么时候都不会出现问题，所以不会上锁！更新数据的时候去判断一下，在此期间是否有人修改过这个数据
- 获取version
- 更新的时候比较version

使用`watch key`监控指定数据，相当于乐观锁加锁。

> 正常执行

```bash
127.0.0.1:6379> set money 100 # 设置余额:100
OK
127.0.0.1:6379> set use 0 # 支出使用:0
OK
127.0.0.1:6379> watch money # 监视money (上锁)
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> DECRBY money 20
QUEUED
127.0.0.1:6379> INCRBY use 20
QUEUED
127.0.0.1:6379> exec # 监视值没有被中途修改，事务正常执行
1) (integer) 80
2) (integer) 20
```

> 测试多线程修改值，使用watch可以当做redis的乐观锁操作（相当于getversion）

我们启动另外一个客户端模拟插队线程。

线程1：

```bash
127.0.0.1:6379> watch money # money上锁
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> DECRBY money 20
QUEUED
127.0.0.1:6379> INCRBY use 20
QUEUED
127.0.0.1:6379> 	# 此时事务并没有执行
```

模拟线程插队，线程2：

```bash
127.0.0.1:6379> INCRBY money 500 # 修改了线程一中监视的money
(integer) 600
12
```

回到线程1，执行事务：

```bash
127.0.0.1:6379> EXEC # 执行之前，另一个线程修改了我们的值，这个时候就会导致事务执行失败
(nil) # 没有结果，说明事务执行失败

127.0.0.1:6379> get money # 线程2 修改生效
"600"
127.0.0.1:6379> get use # 线程1事务执行失败，数值没有被修改
"0"
```

> 解锁获取最新值，然后再加锁进行事务。
>
> `unwatch`进行解锁。

注意：每次提交执行exec后都会自动释放锁，不管是否成功

## 六、Jedis

使用Java来操作Redis，Jedis是Redis官方推荐使用的Java连接redis的客户端。

1. 导入依赖

   ```xml
   <!--导入jredis的包-->
   <dependency>
       <groupId>redis.clients</groupId>
       <artifactId>jedis</artifactId>
       <version>3.2.0</version>
   </dependency>
   <!--fastjson-->
   <dependency>
       <groupId>com.alibaba</groupId>
       <artifactId>fastjson</artifactId>
       <version>1.2.70</version>
   </dependency>
   ```

2. 编码测试

   - 连接数据库

     1. 修改redis的配置文件

        ```bash
        vim /usr/local/bin/myconfig/redis.conf
        1
        ```

        1. 将只绑定本地注释

           [外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-4IRUFJ95-1597890996520)(狂神说 Redis.assets/image-20200813161921480.png)]

        2. 保护模式改为 no

           [外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-oKjIVapw-1597890996521)(狂神说 Redis.assets/image-20200813161939847.png)]

        3. 允许后台运行

           [外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-c2IMvpZL-1597890996522)(狂神说 Redis.assets/image-20200813161954567.png)]

3. 开放端口6379

   ```bash
   firewall-cmd --zone=public --add-port=6379/tcp --permanet
   1
   ```

   重启防火墙服务

   ```bash
   systemctl restart firewalld.service
   1
   ```

   1. 阿里云服务器控制台配置安全组

   2. 重启redis-server

      ```bash
      [root@AlibabaECS bin]# redis-server myconfig/redis.conf 
      1
      ```



- 操作命令

  **TestPing.java**

  ```java
  public class TestPing {
      public static void main(String[] args) {
          Jedis jedis = new Jedis("192.168.xx.xxx", 6379);
          String response = jedis.ping();
          System.out.println(response); // PONG
      }
  }
  ```

- 断开连接

1. **事务**

   ```java
   public class TestTX {
       public static void main(String[] args) {
           Jedis jedis = new Jedis("39.99.xxx.xx", 6379);
   
           JSONObject jsonObject = new JSONObject();
           jsonObject.put("hello", "world");
           jsonObject.put("name", "kuangshen");
           // 开启事务
           Transaction multi = jedis.multi();
           String result = jsonObject.toJSONString();
           // jedis.watch(result)
           try {
               multi.set("user1", result);
               multi.set("user2", result);
               // 执行事务
               multi.exec();
           }catch (Exception e){
               // 放弃事务
               multi.discard();
           } finally {
               // 关闭连接
               System.out.println(jedis.get("user1"));
               System.out.println(jedis.get("user2"));
               jedis.close();
           }
       }
   }
   ```

## 七、SpringBoot整合

1. 导入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

springboot 2.x后 ，原来使用的 Jedis 被 lettuce 替换。

> jedis：采用的直连，多个线程操作的话，是不安全的。如果要避免不安全，使用jedis pool连接池！更像BIO模式
>
> lettuce：采用netty，实例可以在多个线程中共享，不存在线程不安全的情况！可以减少线程数据了，更像NIO模式

我们在学习SpringBoot自动配置的原理时，整合一个组件并进行配置一定会有一个自动配置类xxxAutoConfiguration,并且在spring.factories中也一定能找到这个类的完全限定名。Redis也不例外。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214531573.png)

那么就一定还存在一个RedisProperties类

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214554661.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

之前我们说SpringBoot2.x后默认使用Lettuce来替换Jedis，现在我们就能来验证了。

先看Jedis:

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214607475.png)

@ConditionalOnClass注解中有两个类是默认不存在的，所以Jedis是无法生效的

然后再看Lettuce：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214618179.png)

完美生效。

现在我们回到RedisAutoConfiguratio

![img](https://img-blog.csdnimg.cn/2020051321462777.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

只有两个简单的Bean

- **RedisTemplate**
- **StringRedisTemplate**

当看到xxTemplate时可以对比RestTemplat、SqlSessionTemplate,通过使用这些Template来间接操作组件。那么这俩也不会例外。分别用于操作Redis和Redis中的String数据类型。

在RedisTemplate上也有一个条件注解，说明我们是可以对其进行定制化的

说完这些，我们需要知道如何编写配置文件然后连接Redis，就需要阅读RedisProperties

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214638238.png)

这是一些基本的配置属性。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214649380.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

还有一些连接池相关的配置。注意使用时一定使用Lettuce的连接池。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214700372.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

1. 编写配置文件

   ```properties
   # 配置redis
   spring.redis.host=39.99.xxx.xx
   spring.redis.port=6379
   ```

2. 使用RedisTemplate

   ```java
   @SpringBootTest
   class Redis02SpringbootApplicationTests {
   
       @Autowired
       private RedisTemplate redisTemplate;
   
       @Test
       void contextLoads() {
   
           // redisTemplate 操作不同的数据类型，api和我们的指令是一样的
           // opsForValue 操作字符串 类似String
           // opsForList 操作List 类似List
           // opsForHah
   
           // 除了基本的操作，我们常用的方法都可以直接通过redisTemplate操作，比如事务和基本的CRUD
   
           // 获取连接对象
           //RedisConnection connection = redisTemplate.getConnectionFactory().getConnection();
           //connection.flushDb();
           //connection.flushAll();
   
           redisTemplate.opsForValue().set("mykey","kuangshen");
           System.out.println(redisTemplate.opsForValue().get("mykey"));
       }
   }
   ```

3. 测试结果

   **此时我们回到Redis查看数据时候，惊奇发现全是乱码，可是程序中可以正常输出：**

   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214734520.png)

    这时候就关系到存储对象的序列化问题，在网络中传输的对象也是一样需要序列化，否者就全是乱码。

   我们转到看那个默认的RedisTemplate内部什么样子：

   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214746506.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

   在最开始就能看到几个关于序列化的参数。

   默认的序列化器是采用JDK序列化器

   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214757247.png)

   而默认的RedisTemplate中的所有序列化器都是使用这个序列化器：

   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214809494.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

   后续我们定制RedisTemplate就可以对其进行修改。

   `RedisSerializer`提供了多种序列化方案：

   - 直接调用RedisSerializer的静态方法来返回序列化器，然后set

     ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214818682.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

   - 自己new 相应的实现类，然后set

     ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214827233.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

4. **定制RedisTemplate的模板：**

   我们创建一个Bean加入容器，就会触发RedisTemplate上的条件注解使默认的RedisTemplate失效。

   ```java
   @Configuration
   public class RedisConfig {
   
      @Bean
       public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) throws UnknownHostException {
           // 将template 泛型设置为 <String, Object>
           RedisTemplate<String, Object> template = new RedisTemplate();
           // 连接工厂，不必修改
           template.setConnectionFactory(redisConnectionFactory);
           /*
            * 序列化设置
            */
           // key、hash的key 采用 String序列化方式
           template.setKeySerializer(RedisSerializer.string());
           template.setHashKeySerializer(RedisSerializer.string());
           // value、hash的value 采用 Jackson 序列化方式
           template.setValueSerializer(RedisSerializer.json());
           template.setHashValueSerializer(RedisSerializer.json());
           template.afterPropertiesSet();
           
           return template;
       }
   }
   ```

   这样一来，只要实体类进行了序列化，我们存什么都不会有乱码的担忧了。

   [外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-oc8kJP08-1597890996523)(狂神说 Redis.assets/image-20200817175638086.png)]

## 八、自定义Redis工具类

使用RedisTemplate需要频繁调用`.opForxxx`然后才能进行对应的操作，这样使用起来代码效率低下，工作中一般不会这样使用，而是将这些常用的公共API抽取出来封装成为一个工具类，然后直接使用工具类来间接操作Redis,不但效率高并且易用。

工具类参考博客：

https://www.cnblogs.com/zeng1994/p/03303c805731afc9aa9c60dbbd32a323.html

https://www.cnblogs.com/zhzhlong/p/11434284.html

## 九、Redis.conf

> 容量单位不区分大小写，G和GB有区别

![img](https://img-blog.csdnimg.cn/2020051321485460.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

> 可以使用 include 组合多个配置问题

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214902552.png)

> 网络配置

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214912813.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

> 日志输出级别

![img](https://img-blog.csdnimg.cn/20200513214923678.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

> 日志输出文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214933713.png)

> 持久化规则

由于Redis是基于内存的数据库，需要将数据由内存持久化到文件中

持久化方式：

- RDB
- AOF

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214944964.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

> RDB文件相关

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214955679.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215006207.png)

> 主从复制

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215016371.png)

> Security模块中进行密码设置

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215026143.png)

> 客户端连接相关

```bash
maxclients 10000  最大客户端数量
maxmemory <bytes> 最大内存限制
maxmemory-policy noeviction # 内存达到限制值的处理策略
```

redis 中的**默认**的过期策略是 **volatile-lru** 。

**设置方式**

```bash
config set maxmemory-policy volatile-lru 
1
```

#### **maxmemory-policy 六种方式**

**1、volatile-lru：**只对设置了过期时间的key进行LRU（默认值）

**2、allkeys-lru ：** 删除lru算法的key

**3、volatile-random：**随机删除即将过期key

**4、allkeys-random：**随机删除

**5、volatile-ttl ：** 删除即将过期的

**6、noeviction ：** 永不过期，返回错误

> AOF相关部分

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215037918.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215047999.png)

## 十、持久化—RDB

RDB：Redis Databases

[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-C0mm1D4A-1597890996524)(狂神说 Redis.assets/image-20200818122236614.png)]

### 什么是RDB

------

在指定时间间隔后，将内存中的数据集快照写入数据库 ；在恢复时候，直接读取快照文件，进行数据的恢复 ；

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215126515.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

默认情况下， Redis 将数据库快照保存在名字为 dump.rdb的二进制文件中。文件名可以在配置文件中进行自定义。

### 工作原理

------

在进行 **`RDB`** 的时候，**`redis`** 的主线程是不会做 **`io`** 操作的，主线程会 **`fork`** 一个子线程来完成该操作；

1. Redis 调用forks。同时拥有父进程和子进程。
2. 子进程将数据集写入到一个临时 RDB 文件中。
3. 当子进程完成对新 RDB 文件的写入时，Redis 用新 RDB 文件替换原来的 RDB 文件，并删除旧的 RDB 文件。

这种工作方式使得 Redis 可以从写时复制（copy-on-write）机制中获益(因为是使用子进程进行写操作，而父进程依然可以接收来自客户端的请求。)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215141519.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

### 触发机制

------

1. save的规则满足的情况下，会自动触发rdb原则
2. 执行flushall命令，也会触发我们的rdb原则
3. 退出redis，也会自动产生rdb文件

#### save

使用 `save` 命令，会立刻对当前内存中的数据进行持久化 ,但是会阻塞，也就是不接受其他操作了；

> 由于 `save` 命令是同步命令，会占用Redis的主进程。若Redis数据非常多时，`save`命令执行速度会非常慢，阻塞所有客户端的请求。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215150892.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

#### flushall命令

`flushall` 命令也会触发持久化 ；

#### 触发持久化规则

满足配置条件中的触发条件 ；

> 可以通过配置文件对 Redis 进行设置， 让它在“ N 秒内数据集至少有 M 个改动”这一条件被满足时， 自动进行数据集保存操作。
>
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215205970.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215220858.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

#### bgsave

`bgsave` 是异步进行，进行持久化的时候，`redis` 还可以将继续响应客户端请求 ；

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020051321523151.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

**bgsave和save对比**

| 命令   | save               | bgsave                             |
| ------ | ------------------ | ---------------------------------- |
| IO类型 | 同步               | 异步                               |
| 阻塞？ | 是                 | 是（阻塞发生在fock()，通常非常快） |
| 复杂度 | O(n)               | O(n)                               |
| 优点   | 不会消耗额外的内存 | 不阻塞客户端命令                   |
| 缺点   | 阻塞客户端命令     | 需要fock子进程，消耗内存           |

### 优缺点

**优点：**

1. 适合大规模的数据恢复
2. 对数据的完整性要求不高

**缺点：**

1. 需要一定的时间间隔进行操作，如果redis意外宕机了，这个最后一次修改的数据就没有了。
2. fork进程的时候，会占用一定的内容空间。

## 十一、持久化AOF

**Append Only File**

将我们所有的命令都记录下来，history，恢复的时候就把这个文件全部再执行一遍

[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-Z8wr9lBW-1597890996525)(狂神说 Redis.assets/image-20200818123711375.png)]

> 以日志的形式来记录每个写的操作，将Redis执行过的所有指令记录下来（读操作不记录），只许追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作。

### 什么是AOF

 快照功能（RDB）并不是非常耐久（durable）： 如果 Redis 因为某些原因而造成故障停机， 那么服务器将丢失最近写入、以及未保存到快照中的那些数据。 从 1.1 版本开始， Redis 增加了一种完全耐久的持久化方式： AOF 持久化。

如果要使用AOF，需要修改配置文件：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215247113.png)

`appendonly no yes`则表示启用AOF

默认是不开启的，我们需要手动配置，然后重启redis，就可以生效了！

如果这个aof文件有错位，这时候redis是启动不起来的，我需要修改这个aof文件

redis给我们提供了一个工具`redis-check-aof --fix`

> 优点和缺点

```bash
appendonly yes  # 默认是不开启aof模式的，默认是使用rdb方式持久化的，在大部分的情况下，rdb完全够用
appendfilename "appendonly.aof"

# appendfsync always # 每次修改都会sync 消耗性能
appendfsync everysec # 每秒执行一次 sync 可能会丢失这一秒的数据
# appendfsync no # 不执行 sync ,这时候操作系统自己同步数据，速度最快
```

**优点**

1. 每一次修改都会同步，文件的完整性会更加好
2. 没秒同步一次，可能会丢失一秒的数据
3. 从不同步，效率最高

**缺点**

1. 相对于数据文件来说，aof远远大于rdb，修复速度比rdb慢！
2. Aof运行效率也要比rdb慢，所以我们redis默认的配置就是rdb持久化

## 十二、RDB和AOP选择

### RDB 和 AOF 对比

|            | RDB    | AOF          |
| ---------- | ------ | ------------ |
| 启动优先级 | 低     | 高           |
| 体积       | 小     | 大           |
| 恢复速度   | 快     | 慢           |
| 数据安全性 | 丢数据 | 根据策略决定 |

### 如何选择使用哪种持久化方式？

一般来说， 如果想达到足以媲美 PostgreSQL 的数据安全性， 你应该同时使用两种持久化功能。

如果你非常关心你的数据， 但仍然可以承受数分钟以内的数据丢失， 那么你可以只使用 RDB 持久化。

有很多用户都只使用 AOF 持久化， 但并不推荐这种方式： 因为定时生成 RDB 快照（snapshot）非常便于进行数据库备份， 并且 RDB 恢复数据集的速度也要比 AOF 恢复的速度要快。

## 十三、发布与订阅

Redis 发布订阅(pub/sub)是一种消息通信模式：发送者(pub)发送消息，订阅者(sub)接收消息。

[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-IBT2pjCa-1597890996526)(狂神说 Redis.assets/image-20200818162849693.png)]

下图展示了频道 channel1 ， 以及订阅这个频道的三个客户端 —— client2 、 client5 和 client1 之间的关系：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215523258.png)

当有新消息通过 PUBLISH 命令发送给频道 channel1 时， 这个消息就会被发送给订阅它的三个客户端：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020051321553483.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

### 命令

| 命令                                     | 描述                               |
| ---------------------------------------- | ---------------------------------- |
| `PSUBSCRIBE pattern [pattern..]`         | 订阅一个或多个符合给定模式的频道。 |
| `PUNSUBSCRIBE pattern [pattern..]`       | 退订一个或多个符合给定模式的频道。 |
| `PUBSUB subcommand [argument[argument]]` | 查看订阅与发布系统状态。           |
| `PUBLISH channel message`                | 向指定频道发布消息                 |
| `SUBSCRIBE channel [channel..]`          | 订阅给定的一个或多个频道。         |
| `SUBSCRIBE channel [channel..]`          | 退订一个或多个频道                 |

### 示例

```bash
------------订阅端----------------------
127.0.0.1:6379> SUBSCRIBE sakura # 订阅sakura频道
Reading messages... (press Ctrl-C to quit) # 等待接收消息
1) "subscribe" # 订阅成功的消息
2) "sakura"
3) (integer) 1
1) "message" # 接收到来自sakura频道的消息 "hello world"
2) "sakura"
3) "hello world"
1) "message" # 接收到来自sakura频道的消息 "hello i am sakura"
2) "sakura"
3) "hello i am sakura"

--------------消息发布端-------------------
127.0.0.1:6379> PUBLISH sakura "hello world" # 发布消息到sakura频道
(integer) 1
127.0.0.1:6379> PUBLISH sakura "hello i am sakura" # 发布消息
(integer) 1

-----------------查看活跃的频道------------
127.0.0.1:6379> PUBSUB channels
1) "sakura"
```

### 原理

每个 Redis 服务器进程都维持着一个表示服务器状态的 redis.h/redisServer 结构， 结构的 pubsub_channels 属性是一个字典， 这个字典就用于保存订阅频道的信息，其中，字典的键为正在被订阅的频道， 而字典的值则是一个链表， 链表中保存了所有订阅这个频道的客户端。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020051321554964.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

客户端订阅，就被链接到对应频道的链表的尾部，退订则就是将客户端节点从链表中移除。

### 缺点

1. 如果一个客户端订阅了频道，但自己读取消息的速度却不够快的话，那么不断积压的消息会使redis输出缓冲区的体积变得越来越大，这可能使得redis本身的速度变慢，甚至直接崩溃。
2. 这和数据传输可靠性有关，如果在订阅方断线，那么他将会丢失所有在短线期间发布者发布的消息。

### 应用

1. 消息订阅：公众号订阅，微博关注等等（起始更多是使用消息队列来进行实现）
2. 多人在线聊天室。

稍微复杂的场景，我们就会使用消息中间件MQ处理。

## 十四、主从复制

### 概念

将一台Redis服务器的数据复制到其他Redis上。前者称为主节点（Master/Leader）,后者称为从节点（Slave/Follower）， 数据的复制是单向的！只能由主节点复制到从节点（主节点以写为主、从节点以读为主）。（数据库也类似，主从可以读写分离）

默认情况下，每台Redis都是主节点，一个主节点可以有0个或者多个从节点，但每个从节点只能有一个主节点

### 作用

1. 数据冗余：主从复制实现了数据的热备份，是持久化之外的一种数据冗余的方式。
2. 故障恢复：当主节点故障时，从节点可以暂时替代主节点提供服务，是一种服务冗余的方式
3. 负载均衡：在主从复制的基础上，配合读写分离，由主节点进行写操作，从节点进行读操作，分担服务器的负载；尤其是在多读少写的场景下，通过多个从节点分担负载，提高并发量。
4. 高可用基石：哨兵和集群能够实施的基础

### 为什么使用集群

1. 单台服务器难以负载大量的请求
2. 单台服务器故障率高，系统崩坏概率大
3. 单台服务器内存容量有限。

### 环境配置

我们在讲解配置文件的时候，注意到有一个`replication`模块 (见Redis.conf中第8条)

查看当前库的信息：`info replication`

```bash
127.0.0.1:6379> info replication
# Replication
role:master # 角色
connected_slaves:0 # 从机数量
master_replid:3b54deef5b7b7b7f7dd8acefa23be48879b4fcff
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
```

既然需要启动多个服务，就需要多个配置文件。每个配置文件对应修改以下信息：

- 端口号
- pid文件名
- 日志文件名
- rdb文件名

启动单机多服务集群：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215610163.png)

### 一主二从配置

==默认情况下，每台Redis服务器都是主节点；==我们一般情况下只用配置从机就好了！

认老大！一主（79）二从（80，81）

使用`SLAVEOF host port`就可以为从机配置主机了。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215637483.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

然后主机上也能看到从机的状态：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215645778.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

我们这里是使用命令搭建，是暂时的，==真实开发中应该在从机的配置文件中进行配置，==这样的话是永久的。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215654634.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

### 使用规则

1. 从机只能读，不能写，主机可读可写但是多用于写。

   ```bash
    127.0.0.1:6381> set name sakura # 从机6381写入失败
   (error) READONLY You can't write against a read only replica.
   
   127.0.0.1:6380> set name sakura # 从机6380写入失败
   (error) READONLY You can't write against a read only replica.
   
   127.0.0.1:6379> set name sakura
   OK
   127.0.0.1:6379> get name
   "sakura"
   ```

2. 当主机断电宕机后，默认情况下从机的角色不会发生变化 ，集群中只是失去了写操作，当主机恢复以后，又会连接上从机恢复原状。

3. 当从机断电宕机后，若不是使用配置文件配置的从机，再次启动后作为主机是无法获取之前主机的数据的，若此时重新配置称为从机，又可以获取到主机的所有数据。这里就要提到一个同步原理。

4. 第二条中提到，默认情况下，主机故障后，不会出现新的主机，有两种方式可以产生新的主机：

   - 从机手动执行命令`slaveof no one`,这样执行以后从机会独立出来成为一个主机
   - 使用哨兵模式（自动选举）

> 如果没有老大了，这个时候能不能选择出来一个老大呢？手动！

如果主机断开了连接，我们可以使用`SLAVEOF no one`让自己变成主机！其他的节点就可以手动连接到最新的主节点（手动）！如果这个时候老大修复了，那么就重新连接！

## 十五、哨兵模式

更多信息参考博客：https://www.jianshu.com/p/06ab9daf921d

**主从切换技术的方法是：当主服务器宕机后，需要手动把一台从服务器切换为主服务器，这就需要人工干预，费事费力，还会造成一段时间内服务不可用。**这不是一种推荐的方式，更多时候，我们优先考虑**哨兵模式**。

单机单个哨兵

哨兵的作用：

- 通过发送命令，让Redis服务器返回监控其运行状态，包括主服务器和从服务器。
- 当哨兵监测到master宕机，会自动将slave切换成master，然后通过**发布订阅模式**通知其他的从服务器，修改配置文件，让它们切换主机。

多哨兵模式

哨兵的核心配置

```
sentinel monitor mymaster 127.0.0.1 6379 1
```

- 数字1表示 ：当一个哨兵主观认为主机断开，就可以客观认为主机故障，然后开始选举新的主机。

> 测试

```
redis-sentinel xxx/sentinel.conf
```

成功启动哨兵模式

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215752444.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

此时哨兵监视着我们的主机6379，当我们断开主机后：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215806972.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

> 哨兵模式优缺点

**优点：**

1. 哨兵集群，基于主从复制模式，所有主从复制的优点，它都有
2. 主从可以切换，故障可以转移，系统的可用性更好
3. 哨兵模式是主从模式的升级，手动到自动，更加健壮

**缺点：**

1. Redis不好在线扩容，集群容量一旦达到上限，在线扩容就十分麻烦
2. 实现哨兵模式的配置其实是很麻烦的，里面有很多配置项

> 哨兵模式的全部配置

完整的哨兵模式配置文件 sentinel.conf

```bash
# Example sentinel.conf
 
# 哨兵sentinel实例运行的端口 默认26379
port 26379
 
# 哨兵sentinel的工作目录
dir /tmp
 
# 哨兵sentinel监控的redis主节点的 ip port 
# master-name  可以自己命名的主节点名字 只能由字母A-z、数字0-9 、这三个字符".-_"组成。
# quorum 当这些quorum个数sentinel哨兵认为master主节点失联 那么这时 客观上认为主节点失联了
# sentinel monitor <master-name> <ip> <redis-port> <quorum>
sentinel monitor mymaster 127.0.0.1 6379 1
 
# 当在Redis实例中开启了requirepass foobared 授权密码 这样所有连接Redis实例的客户端都要提供密码
# 设置哨兵sentinel 连接主从的密码 注意必须为主从设置一样的验证密码
# sentinel auth-pass <master-name> <password>
sentinel auth-pass mymaster MySUPER--secret-0123passw0rd
 
 
# 指定多少毫秒之后 主节点没有应答哨兵sentinel 此时 哨兵主观上认为主节点下线 默认30秒
# sentinel down-after-milliseconds <master-name> <milliseconds>
sentinel down-after-milliseconds mymaster 30000
 
# 这个配置项指定了在发生failover主备切换时最多可以有多少个slave同时对新的master进行 同步，
这个数字越小，完成failover所需的时间就越长，
但是如果这个数字越大，就意味着越 多的slave因为replication而不可用。
可以通过将这个值设为 1 来保证每次只有一个slave 处于不能处理命令请求的状态。
# sentinel parallel-syncs <master-name> <numslaves>
sentinel parallel-syncs mymaster 1
 
 
 
# 故障转移的超时时间 failover-timeout 可以用在以下这些方面： 
#1. 同一个sentinel对同一个master两次failover之间的间隔时间。
#2. 当一个slave从一个错误的master那里同步数据开始计算时间。直到slave被纠正为向正确的master那里同步数据时。
#3.当想要取消一个正在进行的failover所需要的时间。  
#4.当进行failover时，配置所有slaves指向新的master所需的最大时间。不过，即使过了这个超时，slaves依然会被正确配置为指向master，但是就不按parallel-syncs所配置的规则来了
# 默认三分钟
# sentinel failover-timeout <master-name> <milliseconds>
sentinel failover-timeout mymaster 180000
 
# SCRIPTS EXECUTION
 
#配置当某一事件发生时所需要执行的脚本，可以通过脚本来通知管理员，例如当系统运行不正常时发邮件通知相关人员。
#对于脚本的运行结果有以下规则：
#若脚本执行后返回1，那么该脚本稍后将会被再次执行，重复次数目前默认为10
#若脚本执行后返回2，或者比2更高的一个返回值，脚本将不会重复执行。
#如果脚本在执行过程中由于收到系统中断信号被终止了，则同返回值为1时的行为相同。
#一个脚本的最大执行时间为60s，如果超过这个时间，脚本将会被一个SIGKILL信号终止，之后重新执行。
 
#通知型脚本:当sentinel有任何警告级别的事件发生时（比如说redis实例的主观失效和客观失效等等），将会去调用这个脚本，
#这时这个脚本应该通过邮件，SMS等方式去通知系统管理员关于系统不正常运行的信息。调用该脚本时，将传给脚本两个参数，
#一个是事件的类型，
#一个是事件的描述。
#如果sentinel.conf配置文件中配置了这个脚本路径，那么必须保证这个脚本存在于这个路径，并且是可执行的，否则sentinel无法正常启动成功。
#通知脚本
# sentinel notification-script <master-name> <script-path>
  sentinel notification-script mymaster /var/redis/notify.sh
 
# 客户端重新配置主节点参数脚本
# 当一个master由于failover而发生改变时，这个脚本将会被调用，通知相关的客户端关于master地址已经发生改变的信息。
# 以下参数将会在调用脚本时传给脚本:
# <master-name> <role> <state> <from-ip> <from-port> <to-ip> <to-port>
# 目前<state>总是“failover”,
# <role>是“leader”或者“observer”中的一个。 
# 参数 from-ip, from-port, to-ip, to-port是用来和旧的master和新的master(即旧的slave)通信的
# 这个脚本应该是通用的，能被多次调用，不是针对性的。
# sentinel client-reconfig-script <master-name> <script-path>
sentinel client-reconfig-script mymaster /var/redis/reconfig.sh
```

## 十六、缓存穿透与雪崩

### 缓存穿透（查不到）

> 概念

在默认情况下，用户请求数据时，会先在缓存(Redis)中查找，若没找到即缓存未命中，再在数据库中进行查找，数量少可能问题不大，可是一旦大量的请求数据（例如秒杀场景）缓存都没有命中的话，就会全部转移到数据库上，造成数据库极大的压力，就有可能导致数据库崩溃。网络安全中也有人恶意使用这种手段进行攻击被称为洪水攻击。

> 解决方案

**布隆过滤器**

对所有可能查询的参数以Hash的形式存储，以便快速确定是否存在这个值，在控制层先进行拦截校验，校验不通过直接打回，减轻了存储系统的压力。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215824722.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

**缓存空对象**

一次请求若在缓存和数据库中都没找到，就在缓存中方一个空对象用于处理后续这个请求。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215836317.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

 这样做有一个缺陷：存储空对象也需要空间，大量的空对象会耗费一定的空间，存储效率并不高。解决这个缺陷的方式就是设置较短过期时间

即使对空值设置了过期时间，还是会存在缓存层和存储层的数据会有一段时间窗口的不一致，这对于需要保持一致性的业务会有影响。

### 缓存击穿（量太大，缓存过期）

> 概念

 相较于缓存穿透，缓存击穿的目的性更强，一个存在的key，在缓存过期的一刻，同时有大量的请求，这些请求都会击穿到DB，造成瞬时DB请求量大、压力骤增。这就是缓存被击穿，只是针对其中某个key的缓存不可用而导致击穿，但是其他的key依然可以使用缓存响应。

 比如热搜排行上，一个热点新闻被同时大量访问就可能导致缓存击穿。

> 解决方案

1. **设置热点数据永不过期**

   这样就不会出现热点数据过期的情况，但是当Redis内存空间满的时候也会清理部分数据，而且此种方案会占用空间，一旦热点数据多了起来，就会占用部分空间。

2. **加互斥锁(分布式锁)**

   在访问key之前，采用SETNX（set if not exists）来设置另一个短期key来锁住当前key的访问，访问结束再删除该短期key。保证同时刻只有一个线程访问。这样对锁的要求就十分高。

### 缓存雪崩

> 概念

大量的key设置了相同的过期时间，导致在缓存在同一时刻全部失效，造成瞬时DB请求量大、压力骤增，引起雪崩。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215850428.jpeg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

> 解决方案

- redis高可用

  redis有可能挂掉，导致雪崩， 多增几台redis，搭建集群。

- 限流降级

  在缓存失效后，通过加锁或者队列来控制读数据库写缓存的线程数量。比如对某个key只允许一个线程查询数据和写缓存，其他线程等待。

- 数据预热

  正式部署之前，先把可能的数据预先访问一遍，这样部分可能大量访问的数据就会加载到缓存中。即将发生大并发访问前手动触发加载缓存不同的key，设置不同的过期时间，让缓存失效的时间点尽量均匀。
  
## 十七.常见问题

  1.(error) MISCONF Redis is configured to save RDB snapshots, but it is currently not able to persist on disk. Commands that may modify the data set are disabled, because this instance is configured to report errors during writes if RDB snapshotting fails (stop-writes-on-bgsave-error option). Please check the Redis logs for details about the RDB error.

  解决 ： CONFIG set stop-writes-on-bgsave-error no

## 十八命令

###  创建



## 应用场景

1.可用于分布式锁

2.存储读数据，短时间不变更的。 经常查询的数据或者热点数据

热点数据：类似于微博热搜，不知道什么成为热点，我们可以自定义规则，比如1秒钟访问5次，咱们就将其加入缓存中。



什么时候考虑需要缓存？

在使用缓存之前，需要确认你的项目是否真的需要缓存。使用缓存会引入的一定的技术复杂度，后文也将会一一介绍这些复杂度。一般来说从两个方面来个是否需要使用缓存:

1. CPU占用:如果你有某些应用需要消耗大量的cpu去计算，比如正则表达式，如果你使用正则表达式比较频繁，而其又占用了很多CPU的话，那你就应该使用缓存将正则表达式的结果给缓存下来。

2. 数据库IO占用:如果你发现你的数据库连接池比较空闲，那么不应该用缓存。但是如果数据库连接池比较繁忙，甚至经常报出连接不够的报警，那么是时候应该考虑缓存了。

   **场景**：曾经有个服务，被很多其他服务调用，其他时间都还好，但是每天早上10点的时候总出现数据库连接池连接不够的报警，经过排查，发现有几个服务选择了在10点做定时任务，大量的请求打过来，DB连接池不够，从而报出连接池不够的报警。这个时候有几个选择，我们可以通过扩容机器来解决，也可以通过增加数据库连接池来解决，但是没有必要增加这些成本，因为只有在10点的时候才会出现这个问题。 后来引入了缓存，不仅解决了这个问题，而且还增加了读的性能。

如果并没有上述两个问题，那么你不必为了增加缓存而缓存

# 题

## 1、是什么

一个高性能的key-value数据库，完全开源免费，一个NOSQL类型数据库，可以解决高并发、高扩展，大数据存储等一系列的问题，是一个非关系型的数据库

## 2、特点

本质上是一个Key-Value类型的内存数据库，很像memcached，整个数据库统统加载在内存中进行操作，定期通过异步操作把数据库数据flush到硬盘上进行保存。因为是纯内存操作，性能非常出色，每秒可以处理超过 10万次读写操作，是已知性能最快的Key-Value DB。

出色之处不仅是性能，支持多种数据结构，此外单个value的最大限制是1GB，不像 memcached只能保存1MB的数据，因此可以用来实现很多有用的功能，比方说用List来做FIFO双向链表，实现一个轻量级的高性能消息队列服务，用Set可以做高性能的tag系统等。另外也可设置expire时间，因此也可以被当作一个功能加强版的memcached来用。

主要缺点是数据库容量受到物理内存的限制，不能用作海量数据的高性能读写，因此Redis适合的场景主要局限在较小数据量的高性能操作和运算上

## 3、好处

 1.速度快，因为数据在内存中，类似于HashMap，HashMap的优势就是查找和操作的时间复杂度都是O(1)

2.支持丰富数据类型，支持string，list，set，sorted set，hash

### String

**常用命令** ：set/get/decr/incr/mget等；

**应用场景** ：最常用，普通的key/value存储都可以归为此类；

实现方式：String在redis内部存储默认就是一个字符串，被redisObject所引用，当遇到incr、decr等操作时会转成数值型进行计算，此时redisObject的encoding字段为int。

### Hash

**常用命令** ：hget/hset/hgetall等

**应用场景** ：存储用户信息对象，其中包括ID、姓名、年龄和生日，通户ID获取姓名或者年龄或者生日；

实现方式：Redis的Hash实际是内部存储的Value为一个HashMap，并提供了直接存取这个Map成员的接口。Key是ID, value是Map。Map的key是成员的属性名，value是属性值。这样对数据的修改和存取都可以直接通过其内部Map的Key(Redis里称内部Map的key为field)，也就是通过 key(用户ID) + field(属性标签) 就可以操作对应属性数据。当前HashMap的实现有两种方式：当HashMap的成员比较少时Redis为了节省内存会采用类似一维数组的方式来紧凑存储，而不会采用真正的HashMap结构，这时对应的value的redisObject的encoding为zipmap，当成员数量增大时会自动转成真正的HashMap,此时redisObject的encoding字段为int。

### List

**常用命令** ：lpush/rpush/lpop/rpop/lrange等；

**应用场景** ：应用场景非常多，也是Redis最重要的数据结构之一，比如推特的关注列表，粉丝列表等都可以用Redis的list结构来实现；

实现方式：Redis list的实现为一个双向链表，即可以支持反向查找和遍历，更方便操作，不过带来了部分额外的内存开销，Redis内部的很多实现，包括发送缓冲队列等也都是用的这个数据结构。

### Set

**常用命令** ：sadd/spop/smembers/sunion等；

**应用场景** ：与list类似，区别在于set可以自动排重，如果需要存储列表数据，又不希望出现重复数据时，用set，并且提供了判断某个成员是否在一个set集合内的重要接口，这个list没提供

实现方式：set 的内部实现是一个 value永远为null的HashMap，实际就是通过计算hash的方式来快速排重的，这也是set能提供判断一个成员是否在集合内的原因。

### Sorted Set

**常用命令** ：zadd/zrange/zrem/zcard等；

**应用场景** ：Redis sorted set的使用场景与set类似，区别是set不是自动有序的，而sorted set可以通过用户额外提供一个优先级(score)的参数来为成员排序，并且是插入有序的，即自动排序。当你需要一个有序的并且不重复的集合列表，那么可以选择sorted set数据结构，比如twitter 的public timeline可以以发表时间作为score来存储，这样获取时就是自动按时间排好序的。

实现方式：Redis sorted set的内部使用HashMap和跳跃表(SkipList)来保证数据的存储和有序，HashMap里放的是成员到score的映射，而跳跃表里存放的是所有的成员，排序依据是HashMap里存的score,使用跳跃表的结构可以获得比较高的查找效率，并且在实现上比较简单。

## 4、相比memcached有哪些优势

1.memcached所有的值均是简单的字符串，redis支持很多数据类型

2.速度比memcached快很多 

3.可以持久化其数据

## 5、Memcache与Redis区别

存储方式： Memecache数据全部存在内存，断电后会挂掉，数据不能超过内存大小。Redis有部份存在硬盘上，能保证数据的持久性

数据类型： Memcache对数据类型支持相对简单。Redis有复杂的数据类型。

底层模型：底层实现方式以及与客户端之间通信的应用协议不一样。Redis直接自己构建了VM 机制 ，因为一般的系统调用系统函数的话，会浪费一定的时间去移动和请求。

## 6、适用场景

Redis最适合所有数据in-momory的场景，如：

**6.1 会话缓存（Session Cache）**

最常用的是会话缓存（session cache）。比其他存储（如Memcached）的优势在于Redis提供持久化。

**6.2 全页缓存（FPC）**

除基本的会话token之外，Redis还提供很简便的FPC平台。回到一致性问题，即使重启了Redis实例，因为有磁盘的持久化，用户也不会看到页面加载速度的下降，这是一个极大改进，类似PHP本地FPC。

**6.3 队列**

Reids在内存存储引擎领域的一大优点是提供 list 和 set 操作，这使得Redis能作为一个很好的消息队列平台来使用。Redis作为队列使用的操作，就类似于本地程序语言（如Python）对 list 的 push/pop 操作。

如果你快速的在Google中搜索“Redis queues”，你马上就能找到大量的开源项目，这些项目的目的就是利用Redis创建非常好的后端工具，以满足各种队列需求。例如，Celery有一个后台就是使用Redis作为broker，你可以从这里去查看。

**6.4 排行榜/计数器**

Redis在内存中对数字进行递增或递减的操作实现的非常好。集合（Set）和有序集合（Sorted Set）也使得我们在执行这些操作的时候变的非常简单，Redis只是正好提供了这两种数据结构。所以，我们要从排序集合中获取到排名最靠前的10个用户–我们称之为“user_scores”，我们只需要像下面一样执行即可：

当然，这是假定你是根据你用户的分数做递增的排序。如果你想返回用户及用户的分数，你需要这样执行：

ZRANGE user_scores 0 10 WITHSCORES

Agora Games就是一个很好的例子，用Ruby实现的，它的排行榜就是使用Redis来存储数据的，你可以在这里看到。

**6.5 发布/订阅**

最后（但肯定不是最不重要的）是Redis的发布/订阅功能。发布/订阅的使用场景确实非常多。

## 7、缓存失效策略和主键失效机制

作为缓存系统都要定期清理无效数据，需要一个主键失效和淘汰策略.

在Redis当中，有生存期的key被称为volatile。在创建缓存时，要为给定的key设置生存期，当key过期的时候（生存期为0），它可能会被删除。

**1、影响生存时间的一些操作**

DEL：删除整个 key 

SET 和 GETSET ：覆盖数据

也就是说，修改key对应的value和使用另外相同的key和value来覆盖以后，当前数据的生存时间不同。

比如说，对 key 执行INCR命令，对列表进行LPUSH命令，或者对哈希表执行HSET命令，都不会修改 key 本身的生存时间。

如果使用RENAME对 key 进行改名，那么改名后的 key的生存时间和改名前一样。

RENAME命令的另一种可能是，尝试将一个带生存时间的 key 改名成另一个带生存时间的 another_key ，这时旧的 another_key (以及它的生存时间)会被删除，然后旧的 key 会改名为 another_key ，因此，新的 another_key 的生存时间也和原本的 key 一样。使用PERSIST命令可以在不删除 key 的情况下，移除 key 的生存时间，让 key 重新成为一个persistent key 。

**2、如何更新生存时间**

可以对一个已经带有生存时间的 key 执行EXPIRE命令，新指定的生存时间会取代旧的生存时间。过期时间的精度已经被控制在1ms之内，主键失效的时间复杂度是O（1），

EXPIRE和TTL命令搭配使用，TTL可以查看key的当前生存时间。设置成功返回 1；当 key 不存在或者不能为 key 设置生存时间时，返回 0 。

最大缓存配置 在 redis 中，允许用户设置最大使用内存大小 server.maxmemory 默认为0，没有指定最大缓存，如果有新的数据添加，超过最大内存，则会使redis崩溃，所以一定要设置。redis 内存数据集大小上升到一定大小的时候，就会实行数据淘汰策略。redis 提供 6种数据淘汰策略：

**volatile-lru：** 从已设置过期时间的数据集（ `server.db\[i\].expires`）中挑选最近最少使用的数据淘汰

**volatile-ttl：** 从已设置过期时间的数据集（ `server.db\[i\].expires`）中挑选将要过期的数据淘汰

**volatile-random：** 从已设置过期时间的数据集（ `server.db\[i\].expires`）中任意选择数据淘汰

**allkeys-lru：** 从数据集（ `server.db\[i\].dict`）中挑选最近最少使用的数据淘汰

**allkeys-random：** 从数据集（ `server.db\[i\].dict`）中任意选择数据淘汰

**no-enviction（驱逐）：** 禁止驱逐数据

注意这里的6种机制，volatile和allkeys规定了是对已设置过期时间的数据集淘汰数据还是从全部数据集淘汰数据，后面的lru、ttl以及random是三种不同的淘汰策略，再加上一种no-enviction永不回收的策略。

使用策略规则：

**1、** 如果数据呈现幂律分布，也就是一部分数据访问频率高，一部分数据访问频率低，则使用allkeys-lru**2、** 如果数据呈现平等分布，也就是所有的数据访问频率都相同，则使用allkeys-random

三种数据淘汰策略：

ttl和random比较容易理解，实现也会比较简单。主要是Lru最近最少使用淘汰策略，设计上会对key 按失效时间排序，然后取最先失效的key进行淘汰

## 8、为什么把所有数据放到内存中

为了达到最快的读写速度将数据都读到内存中，并通过异步的方式将数据写入磁盘。所以redis具有快速和数据持久化的特征。如果不将数据放在内存中，磁盘I/O速度严重影响redis的性能。在内存越来越便宜的今天，redis将会越来越受欢迎。

如果设置了最大使用内存，达到内存限值后不能继续插入新值。

## 9、是单进程单线程的 （？）

利用队列技术将并发访问变为串行访问，消除了传统数据库串行控制的开销

## 10、并发竞争问题如何解决

Redis为单进程单线程模式，采用队列模式将并发访问变为串行访问。Redis本身没有锁的概念，Redis对于多个客户端连接并不存在竞争，但是在Jedis客户端对Redis进行并发访问时会发生连接超时、数据转换错误、阻塞、客户端关闭连接等问题，这些问题均是

由于客户端连接混乱造成。对此有2种解决方法：

**10.1** 客户端角度，为保证每个客户端间正常有序与Redis进行通信，对连接进行池化，同时对客户端读写Redis操作采用内部锁synchronized。

**10.2** 服务器角度，利用setnx实现锁。注：对于第一种，需要应用程序自己处理资源的同步，可以使用的方法比较通俗，可以使用synchronized也可以使用lock；第二种需要用到Redis的setnx命令，但是需要注意一些问题。

## 11、redis常见性能问题和解决方案

## 12、redis事物的了解CAS(check-and-set 操作实现乐观锁 )?

## 13、WATCH命令和基于CAS的乐观锁?

## 14、使用过Redis分布式锁么，它是什么回事？

## 15、假如Redis里面有1亿个key，其中有10w个key是以某个固定的已知的前缀开头的，如果将它们全部找出来？

## 16、使用过Redis做异步队列么，你是怎么用的？

## 17、如果有大量的key需要设置同一时间过期，一般需要注意什么？

## 18、Redis如何做持久化的？

## 19、Pipeline有什么好处，为什么要用pipeline？

## 20、Redis的同步机制了解么？

## 21、是否使用过Redis集群，集群的原理是什么？



## 单线程还是多线程？

4.0 之前是完全单线程

4.0 时引入了多线程，但是额外的线程只是用于后台处理，例如：删除对象，核心流程还是完全单线程的。这也是为什么有些人说 4.0 是单线程的，因为指的是核心流程是单线程。

核心流程指的是 redis 正常处理客户端请求的流程，通常包括：接收命令、解析命令、执行命令、返回结果等。



而在最近，redis 6.0 版本又一次引入了多线程概念，与 4.0 不同的是，这次的多线程会涉及到上述的核心流程。

redis 6.0 中，多线程主要用于网络 I/O 阶段，也就是接收命令和写回结果阶段，而在执行命令阶段，还是由单线程串行执行。由于执行时还是串行，因此无需考虑并发安全问题。

值得注意的时，redis 中的多线程组不会同时存在“读”和“写”，这个多线程组只会同时“读”或者同时“写”。

 

redis 6.0 加入多线程 I/O 之后，处理命令的核心流程如下:

1、当有读事件到来时，主线程将该客户端连接放到全局等待读队列

2、读取数据：1）主线程将等待读队列的客户端连接通过轮询调度算法分配给 I/O 线程处理；2）同时主线程也会自己负责处理一个客户端连接的读事件；3）当主线程处理完该连接的读事件后，会自旋等待所有 I/O 线程处理完毕

3、命令执行：主线程按照事件被加入全局等待读队列的顺序（这边保证了执行顺序是正确的），串行执行客户端命令，然后将客户端连接放到全局等待写队列

4、写回结果：跟等待读队列处理类似，主线程将等待写队列的客户端连接使用轮询调度算法分配给 I/O 线程处理，同时自己也会处理一个，当主线程处理完毕后，会自旋等待所有 I/O 线程处理完毕，最后清空队列。

大致流程图如下：



## 为什么 Redis 是单线程？

在  6.0 之前，redis 的核心操作是单线程。

因为是完全基于内存操作，通常情况下CPU不会是瓶颈，瓶颈最有可能是机器内存大小或者网络带宽。

既然CPU不会成为瓶颈，那就采用单线程，如果使用多线程的话会更复杂，同时需要引入上下文切换、加锁等等，会带来额外的性能消耗。

而随着近些年互联网的不断发展，大家对于缓存的性能要求也越来越高，因此 redis 也开始在逐渐往多线程方向发展。

最近的 6.0 版本就对核心流程引入了多线程，主要用于解决 redis 在网络 I/O 上的性能瓶颈。而对于核心的命令执行阶段，目前还是单线程。 

##  为什么使用单进程、单线程也很快

1、基于内存操作

2、使用了 I/O 多路复用模型，select、epoll 等，基于 reactor 模式开发了自己的网络事件处理器

3、单线程可以避免不必要的上下文切换和竞争条件，减少了这方面的性能消耗。

以上三点是性能高的主要原因，其他的还有一些小优化，例如：对数据结构进行了优化，简单动态字符串、压缩列表等。

## 4、项目使用场景

缓存（核心）、分布式锁（set + lua 脚本）、排行榜（zset）、计数（incrby）、消息队列（stream）、地理位置（geo）、访客统计（hyperloglog）等。

 

## 5、常见数据结构

基础的5种：

String：字符串，最基础的数据类型。

List：列表。

Hash：哈希对象。

Set：集合。

Sorted Set：有序集合，Set 的基础上加了个分值。

高级的4种：

HyperLogLog：通常用于基数统计。使用少量固定大小的内存，来统计集合中唯一元素的数量。统计结果不是精确值，而是一个带有0.81%标准差（standard error）的近似值。所以，HyperLogLog适用于一些对于统计结果精确度要求不是特别高的场景，例如网站的UV统计。

Geo：redis 3.2 版本的新特性。可以将用户给定的地理位置信息储存起来， 并对这些信息进行操作：获取2个位置的距离、根据给定地理位置坐标获取指定范围内的地理位置集合。

Bitmap：位图。

Stream：主要用于消息队列，类似于 kafka，可以认为是 pub/sub 的改进版。提供了消息的持久化和主备复制功能，可以让任何客户端访问任何时刻的数据，并且能记住每一个客户端的访问位置，还能保证消息不丢失。

## 6、Redis 的字符串（SDS）和C语言的字符串区别

![image-20220525112217297](https://raw.githubusercontent.com/yn1007220096/typora/master/picture/202205251122562.png)

## 7、Sorted Set底层数据结构

Sorted Set（有序集合）当前有两种编码：ziplist、skiplist

ziplist：使用压缩列表实现，当保存的元素长度都小于64字节，同时数量小于128时，使用该编码方式，否则会使用 skiplist。这两个参数可以通过 zset-max-ziplist-entries、zset-max-ziplist-value 来自定义修改。

![image-20220525112233417](https://raw.githubusercontent.com/yn1007220096/typora/master/picture/202205251122610.png) 

skiplist：zset实现，一个zset同时包含一个字典（dict）和一个跳跃表（zskiplist）

![image-20220525112251084](https://raw.githubusercontent.com/yn1007220096/typora/master/picture/202205251122328.png) 

## 8、Sorted Set 为什么同时使用字典和跳跃表？

为了提升性能

单独使用字典：在执行范围型操作，比如 zrank、zrange，字典需要进行排序，至少需要 O(NlogN) 的时间复杂度及额外 O(N) 的内存空间。

单独使用跳跃表：根据成员查找分值操作的复杂度从 O(1) 上升为 O(logN)。



## 9、Sorted Set 为什么使用跳跃表，而不是红黑树？

主要有以下几个原因：

1）跳表的性能和红黑树差不多。

2）跳表更容易实现和调试。

 

10、Hash 对象底层结构
Hash 对象当前有两种编码：ziplist、hashtable

ziplist：使用压缩列表实现，每当有新的键值对要加入到哈希对象时，程序会先将保存了键的节点推入到压缩列表的表尾，然后再将保存了值的节点推入到压缩列表表尾。

因此：1）保存了同一键值对的两个节点总是紧挨在一起，保存键的节点在前，保存值的节点在后；2）先添加到哈希对象中的键值对会被放在压缩列表的表头方向，而后来添加的会被放在表尾方向。



 

hashtable：使用字典作为底层实现，哈希对象中的每个键值对都使用一个字典键值来保存，跟 java 中的 HashMap 类似。



 

11、Hash 对象的扩容流程
hash 对象在扩容时使用了一种叫“渐进式 rehash”的方式，步骤如下：

1）计算新表 size、掩码，为新表 ht[1] 分配空间，让字典同时持有 ht[0] 和 ht[1] 两个哈希表。

2）将 rehash 索引计数器变量 rehashidx 的值设置为0，表示 rehash 正式开始。

3）在 rehash 进行期间，每次对字典执行添加、删除、査找、更新操作时，程序除了执行指定的操作以外，还会触发额外的 rehash 操作，在源码中的 _dictRehashStep 方法。

_dictRehashStep：从名字也可以看出来，大意是 rehash 一步，也就是 rehash 一个索引位置。

该方法会从 ht[0] 表的 rehashidx 索引位置上开始向后查找，找到第一个不为空的索引位置，将该索引位置的所有节点 rehash 到 ht[1]，当本次 rehash 工作完成之后，将 ht[0] 索引位置为 rehashidx 的节点清空，同时将 rehashidx 属性的值加一。

4）将 rehash 分摊到每个操作上确实是非常妙的方式，但是万一此时服务器比较空闲，一直没有什么操作，难道 redis 要一直持有两个哈希表吗？

答案当然不是的。我们知道，redis 除了文件事件外，还有时间事件，redis 会定期触发时间事件，这些时间事件用于执行一些后台操作，其中就包含 rehash 操作：当 redis 发现有字典正在进行 rehash 操作时，会花费1毫秒的时间，一起帮忙进行 rehash。

5）随着操作的不断执行，最终在某个时间点上，ht[0] 的所有键值对都会被 rehash 至 ht[1]，此时 rehash 流程完成，会执行最后的清理工作：释放 ht[0] 的空间、将 ht[0] 指向 ht[1]、重置 ht[1]、重置 rehashidx 的值为 -1。

 

12、渐进式 rehash 的优点
渐进式 rehash 的好处在于它采取分而治之的方式，将 rehash 键值对所需的计算工作均摊到对字典的每个添加、删除、查找和更新操作上，从而避免了集中式 rehash 而带来的庞大计算量。

在进行渐进式 rehash 的过程中，字典会同时使用 ht[0] 和 ht[1] 两个哈希表， 所以在渐进式 rehash 进行期间，字典的删除、査找、更新等操作会在两个哈希表上进行。例如，要在字典里面査找一个键的话，程序会先在 ht[0] 里面进行査找，如果没找到的话，就会继续到 ht[1] 里面进行査找，诸如此类。

另外，在渐进式 rehash 执行期间，新增的键值对会被直接保存到 ht[1], ht[0] 不再进行任何添加操作，这样就保证了 ht[0] 包含的键值对数量会只减不增，并随着 rehash 操作的执行而最终变成空表。

 

13、rehash 流程在数据量大的时候会有什么问题吗（Hash 对象的扩容流程在数据量大的时候会有什么问题吗）
1）扩容期开始时，会先给 ht[1] 申请空间，所以在整个扩容期间，会同时存在 ht[0] 和 ht[1]，会占用额外的空间。

2）扩容期间同时存在 ht[0] 和 ht[1]，查找、删除、更新等操作有概率需要操作两张表，耗时会增加。

3）redis 在内存使用接近 maxmemory 并且有设置驱逐策略的情况下，出现 rehash 会使得内存占用超过 maxmemory，触发驱逐淘汰操作，导致 master/slave 均有有大量的 key 被驱逐淘汰，从而出现 master/slave 主从不一致。

 

14、Redis 的网络事件处理器（Reactor 模式）
redis 基于 reactor 模式开发了自己的网络事件处理器，由4个部分组成：套接字、I/O 多路复用程序、文件事件分派器（dispatcher）、以及事件处理器。



套接字：socket 连接，也就是客户端连接。当一个套接字准备好执行连接、写入、读取、关闭等操作时， 就会产生一个相应的文件事件。因为一个服务器通常会连接多个套接字， 所以多个文件事件有可能会并发地出现。

I/O 多路复用程序：提供 select、epoll、evport、kqueue 的实现，会根据当前系统自动选择最佳的方式。负责监听多个套接字，当套接字产生事件时，会向文件事件分派器传送那些产生了事件的套接字。当多个文件事件并发出现时， I/O 多路复用程序会将所有产生事件的套接字都放到一个队列里面，然后通过这个队列，以有序、同步、每次一个套接字的方式向文件事件分派器传送套接字：当上一个套接字产生的事件被处理完毕之后，才会继续传送下一个套接字。

文件事件分派器：接收 I/O 多路复用程序传来的套接字， 并根据套接字产生的事件的类型， 调用相应的事件处理器。

事件处理器：事件处理器就是一个个函数， 定义了某个事件发生时， 服务器应该执行的动作。例如：建立连接、命令查询、命令写入、连接关闭等等。

 

# 15、Redis 删除过期键的策略（缓存失效策略、数据过期策略）

定时删除：设置键过期时间的同时，创建一个定时器，让定时器在键的过期时间来临时，立即执行对键的删除操作。对内存最友好，对 CPU 时间最不友好。

惰性删除：放任键过期不管，但是每次获取键时，都检査键是否过期，如果过期的话，就删除该键；如果没有过期，就返回该键。对 CPU 友好，对内存最不友好。

定期删除：每隔一段时间，默认100ms，程序就对数据库进行一次检査，删除里面的过期键。至于要删除多少过期键，以及要检査多少个数据库，则由算法决定。前两种策略的折中，对 CPU 时间和内存的友好程度较平衡。

Redis 使用惰性删除和定期删除。

 

16、Redis 的内存淘汰（驱逐）策略
当 redis 的内存空间（maxmemory 参数配置）已经用满时，redis 将根据配置的驱逐策略（maxmemory-policy 参数配置），进行相应的动作。

网上很多资料都是写 6 种，但是其实当前 redis 的淘汰策略已经有 8 种了，多余的两种是 Redis 4.0 新增的，基于 LFU（Least Frequently Used）算法实现的。

noeviction：默认策略，不淘汰任何 key，直接返回错误
allkeys-lru：在所有的 key 中，使用 LRU 算法淘汰部分 key
allkeys-lfu：在所有的 key 中，使用 LFU 算法淘汰部分 key，该算法于 Redis 4.0 新增
allkeys-random：在所有的 key 中，随机淘汰部分 key
volatile-lru：在设置了过期时间的 key 中，使用 LRU 算法淘汰部分 key
volatile-lfu：在设置了过期时间的 key 中，使用 LFU 算法淘汰部分 key，该算法于 Redis 4.0 新增
volatile-random：在设置了过期时间的 key 中，随机淘汰部分 key
volatile-ttl：在设置了过期时间的 key 中，挑选 TTL（time to live，剩余时间）短的 key 淘汰


17、Redis 的 LRU 算法怎么实现的？
Redis 在 redisObject 结构体中定义了一个长度 24 bit 的 unsigned 类型的字段（unsigned lru:LRU_BITS），在 LRU 算法中用来存储对象最后一次被命令程序访问的时间。

具体的 LRU 算法经历了两个版本。

版本1：随机选取 N 个淘汰法。

最初 Redis 是这样实现的：随机选 N（默认5） 个 key，把空闲时间（idle time）最大的那个 key 移除。这边的 N 可通过 maxmemory-samples 配置项修改。

就是这么简单，简单得让人不敢相信了，而且十分有效。

但是这个算法有个明显的缺点：每次都是随机从 N 个里选择 1 个，并没有利用前一轮的历史信息。其实在上一轮移除 key 的过程中，其实是知道了 N 个 key 的 idle time 的情况的，那在下一轮移除 key 时，其实可以利用上一轮的这些信息。这也是 Redis 3.0 的优化思想。

 

版本2：Redis 3.0 对 LRU 算法进行改进，引入了缓冲池（pool，默认16）的概念。

当每一轮移除 key 时，拿到了 N（默认5）个 key 的 idle time，遍历处理这 N 个 key，如果 key 的 idle time 比 pool 里面的 key 的 idle time 还要大，就把它添加到 pool 里面去。

当 pool 放满之后，每次如果有新的 key 需要放入，需要将 pool 中 idle time 最小的一个 key 移除。这样相当于 pool 里面始终维护着还未被淘汰的 idle time 最大的 16 个 key。

当我们每轮要淘汰的时候，直接从 pool 里面取出 idle time 最大的 key（只取1个），将之淘汰掉。

整个流程相当于随机取 5 个 key 放入 pool，然后淘汰 pool 中空闲时间最大的 key，然后再随机取 5 个 key放入 pool，继续淘汰 pool 中空闲时间最大的 key，一直持续下去。

在进入淘汰前会计算出需要释放的内存大小，然后就一直循环上述流程，直至释放足够的内存。

 

18、Redis 的持久化机制有哪几种，各自的实现原理和优缺点？
Redis 的持久化机制有：RDB、AOF、混合持久化（RDB+AOF，Redis 4.0引入）。

1）RDB

描述：类似于快照。在某个时间点，将 Redis 在内存中的数据库状态（数据库的键值对等信息）保存到磁盘里面。RDB 持久化功能生成的 RDB 文件是经过压缩的二进制文件。

命令：有两个 Redis 命令可以用于生成 RDB 文件，一个是 SAVE，另一个是 BGSAVE。

开启：使用 save point 配置，满足 save point 条件后会触发 BGSAVE 来存储一次快照，这边的 save point 检查就是在上文提到的 serverCron 中进行。

save point 格式：save <seconds> <changes>，含义是 Redis 如果在 seconds 秒内数据发生了 changes 次改变，就保存快照文件。例如 Redis 默认就配置了以下3个：

save 900 1 #900秒内有1个key发生了变化，则触发保存RDB文件
save 300 10 #300秒内有10个key发生了变化，则触发保存RDB文件
save 60 10000 #60秒内有10000个key发生了变化，则触发保存RDB文件
关闭：1）注释掉所有save point 配置可以关闭 RDB 持久化。2）在所有 save point 配置后增加：save ""，该配置可以删除所有之前配置的 save point。

save ""
SAVE：生成 RDB 快照文件，但是会阻塞主进程，服务器将无法处理客户端发来的命令请求，所以通常不会直接使用该命令。

BGSAVE：fork 子进程来生成 RDB 快照文件，阻塞只会发生在 fork 子进程的时候，之后主进程可以正常处理请求，详细过程如下图：



fork：在 Linux 系统中，调用 fork() 时，会创建出一个新进程，称为子进程，子进程会拷贝父进程的 page table。如果进程占用的内存越大，进程的 page table 也会越大，那么 fork 也会占用更多的时间。如果 Redis 占用的内存很大，那么在 fork 子进程时，则会出现明显的停顿现象。

RDB 的优点

1）RDB 文件是是经过压缩的二进制文件，占用空间很小，它保存了 Redis 某个时间点的数据集，很适合用于做备份。 比如说，你可以在最近的 24 小时内，每小时备份一次 RDB 文件，并且在每个月的每一天，也备份一个 RDB 文件。这样的话，即使遇上问题，也可以随时将数据集还原到不同的版本。

2）RDB 非常适用于灾难恢复（disaster recovery）：它只有一个文件，并且内容都非常紧凑，可以（在加密后）将它传送到别的数据中心。

3）RDB 可以最大化 redis 的性能。父进程在保存 RDB 文件时唯一要做的就是 fork 出一个子进程，然后这个子进程就会处理接下来的所有保存工作，父进程无须执行任何磁盘 I/O 操作。

4）RDB 在恢复大数据集时的速度比 AOF 的恢复速度要快。

RDB 的缺点

1）RDB 在服务器故障时容易造成数据的丢失。RDB 允许我们通过修改 save point 配置来控制持久化的频率。但是，因为 RDB 文件需要保存整个数据集的状态， 所以它是一个比较重的操作，如果频率太频繁，可能会对 Redis 性能产生影响。所以通常可能设置至少5分钟才保存一次快照，这时如果 Redis 出现宕机等情况，则意味着最多可能丢失5分钟数据。

2）RDB 保存时使用 fork 子进程进行数据的持久化，如果数据比较大的话，fork 可能会非常耗时，造成 Redis 停止处理服务N毫秒。如果数据集很大且 CPU 比较繁忙的时候，停止服务的时间甚至会到一秒。

3）Linux fork 子进程采用的是 copy-on-write 的方式。在 Redis 执行 RDB 持久化期间，如果 client 写入数据很频繁，那么将增加 Redis 占用的内存，最坏情况下，内存的占用将达到原先的2倍。刚 fork 时，主进程和子进程共享内存，但是随着主进程需要处理写操作，主进程需要将修改的页面拷贝一份出来，然后进行修改。极端情况下，如果所有的页面都被修改，则此时的内存占用是原先的2倍。


2）AOF

描述：保存 Redis 服务器所执行的所有写操作命令来记录数据库状态，并在服务器启动时，通过重新执行这些命令来还原数据集。

开启：AOF 持久化默认是关闭的，可以通过配置：appendonly yes 开启。

关闭：使用配置 appendonly no 可以关闭 AOF 持久化。

AOF 持久化功能的实现可以分为三个步骤：命令追加、文件写入、文件同步。

命令追加：当 AOF 持久化功能打开时，服务器在执行完一个写命令之后，会将被执行的写命令追加到服务器状态的 aof 缓冲区（aof_buf）的末尾。

文件写入与文件同步：可能有人不明白为什么将 aof_buf 的内容写到磁盘上需要两步操作，这边简单解释一下。

Linux 操作系统中为了提升性能，使用了页缓存（page cache）。当我们将 aof_buf 的内容写到磁盘上时，此时数据并没有真正的落盘，而是在 page cache 中，为了将 page cache 中的数据真正落盘，需要执行 fsync / fdatasync 命令来强制刷盘。这边的文件同步做的就是刷盘操作，或者叫文件刷盘可能更容易理解一些。

在文章开头，我们提过 serverCron 时间事件中会触发 flushAppendOnlyFile 函数，该函数会根据服务器配置的 appendfsync 参数值，来决定是否将 aof_buf 缓冲区的内容写入和保存到 AOF 文件。

appendfsync 参数有三个选项：

always：每处理一个命令都将 aof_buf 缓冲区中的所有内容写入并同步到AOF 文件，即每个命令都刷盘。
everysec：将 aof_buf 缓冲区中的所有内容写入到 AOF 文件，如果上次同步 AOF 文件的时间距离现在超过一秒钟， 那么再次对 AOF 文件进行同步， 并且这个同步操作是异步的，由一个后台线程专门负责执行，即每秒刷盘1次。
no：将 aof_buf 缓冲区中的所有内容写入到 AOF 文件， 但并不对 AOF 文件进行同步， 何时同步由操作系统来决定。即不执行刷盘，让操作系统自己执行刷盘。
AOF 的优点

AOF 比 RDB可靠。你可以设置不同的 fsync 策略：no、everysec 和 always。默认是 everysec，在这种配置下，redis 仍然可以保持良好的性能，并且就算发生故障停机，也最多只会丢失一秒钟的数据。
AOF文件是一个纯追加的日志文件。即使日志因为某些原因而包含了未写入完整的命令（比如写入时磁盘已满，写入中途停机等等）， 我们也可以使用 redis-check-aof 工具也可以轻易地修复这种问题。
当 AOF文件太大时，Redis 会自动在后台进行重写：重写后的新 AOF 文件包含了恢复当前数据集所需的最小命令集合。整个重写是绝对安全，因为重写是在一个新的文件上进行，同时 Redis 会继续往旧的文件追加数据。当新文件重写完毕，Redis 会把新旧文件进行切换，然后开始把数据写到新文件上。
AOF 文件有序地保存了对数据库执行的所有写入操作以 Redis 协议的格式保存， 因此 AOF 文件的内容非常容易被人读懂， 对文件进行分析（parse）也很轻松。如果你不小心执行了 FLUSHALL 命令把所有数据刷掉了，但只要 AOF 文件没有被重写，那么只要停止服务器， 移除 AOF 文件末尾的 FLUSHALL 命令， 并重启 Redis ， 就可以将数据集恢复到 FLUSHALL 执行之前的状态。
AOF 的缺点

对于相同的数据集，AOF 文件的大小一般会比 RDB 文件大。
根据所使用的 fsync 策略，AOF 的速度可能会比 RDB 慢。通常 fsync 设置为每秒一次就能获得比较高的性能，而关闭 fsync 可以让 AOF 的速度和 RDB 一样快。
AOF 在过去曾经发生过这样的 bug ：因为个别命令的原因，导致 AOF 文件在重新载入时，无法将数据集恢复成保存时的原样。（举个例子，阻塞命令 BRPOPLPUSH 就曾经引起过这样的 bug ） 。虽然这种 bug 在 AOF 文件中并不常见， 但是相较而言， RDB 几乎是不可能出现这种 bug 的。

3）混合持久化

描述：混合持久化并不是一种全新的持久化方式，而是对已有方式的优化。混合持久化只发生于 AOF 重写过程。使用了混合持久化，重写后的新 AOF 文件前半段是 RDB 格式的全量数据，后半段是 AOF 格式的增量数据。

整体格式为：[RDB file][AOF tail]

开启：混合持久化的配置参数为 aof-use-rdb-preamble，配置为 yes 时开启混合持久化，在 redis 4 刚引入时，默认是关闭混合持久化的，但是在 redis 5 中默认已经打开了。

关闭：使用 aof-use-rdb-preamble no 配置即可关闭混合持久化。

混合持久化本质是通过 AOF 后台重写（bgrewriteaof 命令）完成的，不同的是当开启混合持久化时，fork 出的子进程先将当前全量数据以 RDB 方式写入新的 AOF 文件，然后再将 AOF 重写缓冲区（aof_rewrite_buf_blocks）的增量命令以 AOF 方式写入到文件，写入完成后通知主进程将新的含有 RDB 格式和 AOF 格式的 AOF 文件替换旧的的 AOF 文件。

优点：结合 RDB 和 AOF 的优点, 更快的重写和恢复。

缺点：AOF 文件里面的 RDB 部分不再是 AOF 格式，可读性差。

 

19、为什么需要 AOF 重写
AOF 持久化是通过保存被执行的写命令来记录数据库状态的，随着写入命令的不断增加，AOF 文件中的内容会越来越多，文件的体积也会越来越大。

如果不加以控制，体积过大的 AOF 文件可能会对 Redis 服务器、甚至整个宿主机造成影响，并且 AOF 文件的体积越大，使用 AOF 文件来进行数据还原所需的时间就越多。

举个例子， 如果你对一个计数器调用了 100 次 INCR ， 那么仅仅是为了保存这个计数器的当前值， AOF 文件就需要使用 100 条记录。

然而在实际上， 只使用一条 SET 命令已经足以保存计数器的当前值了， 其余 99 条记录实际上都是多余的。

为了处理这种情况， Redis 引入了 AOF 重写：可以在不打断服务端处理请求的情况下， 对 AOF 文件进行重建（rebuild）。

 

20、介绍下 AOF 重写的过程、AOF 后台重写存在的问题、如何解决 AOF 后台重写存在的数据不一致问题
描述：Redis 生成新的 AOF 文件来代替旧 AOF 文件，这个新的 AOF 文件包含重建当前数据集所需的最少命令。具体过程是遍历所有数据库的所有键，从数据库读取键现在的值，然后用一条命令去记录键值对，代替之前记录这个键值对的多条命令。

命令：有两个 Redis 命令可以用于触发 AOF 重写，一个是 BGREWRITEAOF 、另一个是  REWRITEAOF 命令；

开启：AOF 重写由两个参数共同控制，auto-aof-rewrite-percentage 和 auto-aof-rewrite-min-size，同时满足这两个条件，则触发 AOF 后台重写 BGREWRITEAOF。

// 当前AOF文件比上次重写后的AOF文件大小的增长比例超过100
auto-aof-rewrite-percentage 100 
// 当前AOF文件的文件大小大于64MB
auto-aof-rewrite-min-size 64mb
关闭：auto-aof-rewrite-percentage 0，指定0的百分比，以禁用自动AOF重写功能。

auto-aof-rewrite-percentage 0
REWRITEAOF：进行 AOF 重写，但是会阻塞主进程，服务器将无法处理客户端发来的命令请求，通常不会直接使用该命令。

BGREWRITEAOF：fork 子进程来进行 AOF 重写，阻塞只会发生在 fork 子进程的时候，之后主进程可以正常处理请求。

REWRITEAOF 和 BGREWRITEAOF 的关系与 SAVE 和 BGSAVE 的关系类似。

 

AOF 后台重写存在的问题

AOF 后台重写使用子进程进行从写，解决了主进程阻塞的问题，但是仍然存在另一个问题：子进程在进行 AOF 重写期间，服务器主进程还需要继续处理命令请求，新的命令可能会对现有的数据库状态进行修改，从而使得当前的数据库状态和重写后的 AOF 文件保存的数据库状态不一致。

 

如何解决 AOF 后台重写存在的数据不一致问题

为了解决上述问题，Redis 引入了 AOF 重写缓冲区（aof_rewrite_buf_blocks），这个缓冲区在服务器创建子进程之后开始使用，当 Redis 服务器执行完一个写命令之后，它会同时将这个写命令追加到 AOF 缓冲区和 AOF 重写缓冲区。

这样一来可以保证：

1、现有 AOF 文件的处理工作会如常进行。这样即使在重写的中途发生停机，现有的 AOF 文件也还是安全的。

2、从创建子进程开始，也就是 AOF 重写开始，服务器执行的所有写命令会被记录到 AOF 重写缓冲区里面。

这样，当子进程完成 AOF 重写工作后，父进程会在 serverCron 中检测到子进程已经重写结束，则会执行以下工作：

1、将 AOF 重写缓冲区中的所有内容写入到新 AOF 文件中，这时新 AOF 文件所保存的数据库状态将和服务器当前的数据库状态一致。

2、对新的 AOF 文件进行改名，原子的覆盖现有的 AOF 文件，完成新旧两个 AOF 文件的替换。

之后，父进程就可以继续像往常一样接受命令请求了。

 

21、RDB、AOF、混合持久，我应该用哪一个？
一般来说， 如果想尽量保证数据安全性， 你应该同时使用 RDB 和 AOF 持久化功能，同时可以开启混合持久化。

如果你非常关心你的数据， 但仍然可以承受数分钟以内的数据丢失， 那么你可以只使用 RDB 持久化。

如果你的数据是可以丢失的，则可以关闭持久化功能，在这种情况下，Redis 的性能是最高的。

使用 Redis 通常都是为了提升性能，而如果为了不丢失数据而将 appendfsync  设置为 always 级别时，对 Redis 的性能影响是很大的，在这种不能接受数据丢失的场景，其实可以考虑直接选择 MySQL 等类似的数据库。

 

22、同时开启RDB和AOF，服务重启时如何加载
简单来说，如果同时启用了 AOF 和 RDB，Redis 重新启动时，会使用 AOF 文件来重建数据集，因为通常来说， AOF 的数据会更完整。

而在引入了混合持久化之后，使用 AOF 重建数据集时，会通过文件开头是否为“REDIS”来判断是否为混合持久化。

完整流程如下图所示：



 

23、Redis 怎么保证高可用、有哪些集群模式
主从复制、哨兵模式、集群模式。

 

24、主从复制
在当前最新的 Redis 6.0 中，主从复制的完整过程如下：

1）开启主从复制

通常有以下三种方式：

在 slave 直接执行命令：slaveof <masterip> <masterport>
在 slave 配置文件中加入：slaveof <masterip> <masterport>
使用启动命令：--slaveof <masterip> <masterport>
注：在 Redis 5.0 之后，slaveof 相关命令和配置已经被替换成 replicaof，例如 replicaof <masterip> <masterport>。为了兼容旧版本，通过配置的方式仍然支持 slaveof，但是通过命令的方式则不行了。

2）建立套接字（socket）连接

slave 将根据指定的 IP 地址和端口，向 master 发起套接字（socket）连接，master 在接受（accept） slave 的套接字连接之后，为该套接字创建相应的客户端状态，此时连接建立完成。

3）发送PING命令

slave 向 master 发送一个 PING 命令，以检査套接字的读写状态是否正常、 master 能否正常处理命令请求。

4）身份验证

slave 向 master 发送 AUTH password 命令来进行身份验证。

5）发送端口信息

在身份验证通过后后， slave 将向 master 发送自己的监听端口号， master 收到后记录在 slave 所对应的客户端状态的 slave_listening_port 属性中。

6）发送IP地址

如果配置了 slave_announce_ip，则 slave 向 master 发送 slave_announce_ip 配置的 IP 地址， master 收到后记录在 slave 所对应的客户端状态的 slave_ip 属性。

该配置是用于解决服务器返回内网 IP 时，其他服务器无法访问的情况。可以通过该配置直接指定公网 IP。

7）发送CAPA

CAPA 全称是 capabilities，这边表示的是同步复制的能力。slave 会在这一阶段发送 capa 告诉 master 自己具备的（同步）复制能力， master 收到后记录在 slave 所对应的客户端状态的 slave_capa 属性。

8）数据同步

slave 将向 master 发送 PSYNC 命令， master 收到该命令后判断是进行部分重同步还是完整重同步，然后根据策略进行数据的同步。

9）命令传播

当完成了同步之后，就会进入命令传播阶段，这时 master 只要一直将自己执行的写命令发送给 slave ，而 slave 只要一直接收并执行 master 发来的写命令，就可以保证 master 和 slave 一直保持一致了。


以部分重同步为例，主从复制的核心步骤流程图如下：



 

25、哨兵
哨兵（Sentinel） 是 Redis 的高可用性解决方案：由一个或多个 Sentinel 实例组成的 Sentinel 系统可以监视任意多个主服务器，以及这些主服务器属下的所有从服务器。

Sentinel 可以在被监视的主服务器进入下线状态时，自动将下线主服务器的某个从服务器升级为新的主服务器，然后由新的主服务器代替已下线的主服务器继续处理命令请求。

1）哨兵故障检测

检查主观下线状态

在默认情况下，Sentinel 会以每秒一次的频率向所有与它创建了命令连接的实例（包括主服务器、从服务器、其他 Sentinel 在内）发送 PING 命令，并通过实例返回的 PING 命令回复来判断实例是否在线。

如果一个实例在 down-after-miliseconds 毫秒内，连续向 Sentinel 返回无效回复，那么 Sentinel 会修改这个实例所对应的实例结构，在结构的 flags 属性中设置 SRI_S_DOWN 标识，以此来表示这个实例已经进入主观下线状态。

检查客观下线状态

当 Sentinel 将一个主服务器判断为主观下线之后，为了确定这个主服务器是否真的下线了，它会向同样监视这一服务器的其他 Sentinel 进行询问，看它们是否也认为主服务器已经进入了下线状态（可以是主观下线或者客观下线）。

当 Sentinel 从其他 Sentinel 那里接收到足够数量（quorum，可配置）的已下线判断之后，Sentinel 就会将服务器置为客观下线，在 flags 上打上 SRI_O_DOWN 标识，并对主服务器执行故障转移操作。

2）哨兵故障转移流程

当哨兵监测到某个主节点客观下线之后，就会开始故障转移流程。核心流程如下：

发起一次选举，选举出领头 Sentinel
领头 Sentinel 在已下线主服务器的所有从服务器里面，挑选出一个从服务器，并将其升级为新的主服务器。
领头 Sentinel 将剩余的所有从服务器改为复制新的主服务器。
领头 Sentinel 更新相关配置信息，当这个旧的主服务器重新上线时，将其设置为新的主服务器的从服务器。


26、集群模式
哨兵模式最大的缺点就是所有的数据都放在一台服务器上，无法较好的进行水平扩展。

为了解决哨兵模式存在的问题，集群模式应运而生。在高可用上，集群基本是直接复用的哨兵模式的逻辑，并且针对水平扩展进行了优化。

集群模式具备的特点如下：

采取去中心化的集群模式，将数据按槽存储分布在多个 Redis 节点上。集群共有 16384 个槽，每个节点负责处理部分槽。
使用 CRC16 算法来计算 key 所属的槽：crc16(key,keylen) & 16383。
所有的 Redis 节点彼此互联，通过 PING-PONG 机制来进行节点间的心跳检测。
分片内采用一主多从保证高可用，并提供复制和故障恢复功能。在实际使用中，通常会将主从分布在不同机房，避免机房出现故障导致整个分片出问题，下面的架构图就是这样设计的。
客户端与 Redis 节点直连，不需要中间代理层（proxy）。客户端不需要连接集群所有节点，连接集群中任何一个可用节点即可。
集群的架构图如下所示：



 

27、集群选举
故障转移的第一步就是选举出新的主节点，以下是集群选举新的主节点的方法：

1）当从节点发现自己正在复制的主节点进入已下线状态时，会发起一次选举：将 currentEpoch（配置纪元）加1，然后向集群广播一条 CLUSTERMSG_TYPE_FAILOVER_AUTH_REQUEST 消息，要求所有收到这条消息、并且具有投票权的主节点向这个从节点投票。

2）其他节点收到消息后，会判断是否要给发送消息的节点投票，判断流程如下：

当前节点是 slave，或者当前节点是 master，但是不负责处理槽，则当前节点没有投票权，直接返回。
请求节点的 currentEpoch 小于当前节点的 currentEpoch，校验失败返回。因为发送者的状态与当前集群状态不一致，可能是长时间下线的节点刚刚上线，这种情况下，直接返回即可。
当前节点在该 currentEpoch 已经投过票，校验失败返回。
请求节点是 master，校验失败返回。
请求节点的 master 为空，校验失败返回。
请求节点的 master 没有故障，并且不是手动故障转移，校验失败返回。因为手动故障转移是可以在 master 正常的情况下直接发起的。
上一次为该master的投票时间，在cluster_node_timeout的2倍范围内，校验失败返回。这个用于使获胜从节点有时间将其成为新主节点的消息通知给其他从节点，从而避免另一个从节点发起新一轮选举又进行一次没必要的故障转移
请求节点宣称要负责的槽位，是否比之前负责这些槽位的节点，具有相等或更大的 configEpoch，如果不是，校验失败返回。
如果通过以上所有校验，那么主节点将向要求投票的从节点返回一条 CLUSTERMSG_TYPE_FAILOVER_AUTH_ACK 消息，表示这个主节点支持从节点成为新的主节点。

3）每个参与选举的从节点都会接收 CLUSTERMSG_TYPE_FAILOVER_AUTH_ACK 消息，并根据自己收到了多少条这种消息来统计自己获得了多少个主节点的支持。

4）如果集群里有N个具有投票权的主节点，那么当一个从节点收集到大于等于N/2+1 张支持票时，这个从节点就会当选为新的主节点。因为在每一个配置纪元里面，每个具有投票权的主节点只能投一次票，所以如果有 N个主节点进行投票，那么具有大于等于 N/2+1 张支持票的从节点只会有一个，这确保了新的主节点只会有一个。

5）如果在一个配置纪元里面没有从节点能收集到足够多的支持票，那么集群进入一个新的配置纪元，并再次进行选举，直到选出新的主节点为止。

这个选举新主节点的方法和选举领头 Sentinel 的方法非常相似，因为两者都是基于 Raft 算法的领头选举（leader election）方法来实现的。

 

28、如何保证集群在线扩容的安全性？（Redis 集群要增加分片，槽的迁移怎么保证无损）
例如：集群已经对外提供服务，原来有3分片，准备新增2个分片，怎么在不下线的情况下，无损的从原有的3个分片指派若干个槽给这2个分片？

Redis 使用了 ASK 错误来保证在线扩容的安全性。

在槽的迁移过程中若有客户端访问，依旧先访问源节点，源节点会先在自己的数据库里面査找指定的键，如果找到的话，就直接执行客户端发送的命令。

如果没找到，说明该键可能已经被迁移到目标节点了，源节点将向客户端返回一个 ASK 错误，该错误会指引客户端转向正在导入槽的目标节点，并再次发送之前想要执行的命令，从而获取到结果。

ASK错误

在进行重新分片期间，源节点向目标节点迁移一个槽的过程中，可能会出现这样一种情况：属于被迁移槽的一部分键值对保存在源节点里面，而另一部分键值对则保存在目标节点里面。

当客户端向源节点发送一个与数据库键有关的命令，并且命令要处理的数据库键恰好就属于正在被迁移的槽时。源节点会先在自己的数据库里面査找指定的键，如果找到的话，就直接执行客户端发送的命令。

否则，这个键有可能已经被迁移到了目标节点，源节点将向客户端返回一个 ASK 错误，指引客户端转向正在导入槽的目标节点，并再次发送之前想要执行的命令，从而获取到结果。

 

29、Redis 事务的实现
一个事务从开始到结束通常会经历以下3个阶段：

1）事务开始：multi 命令将执行该命令的客户端从非事务状态切换至事务状态，底层通过 flags 属性标识。

2）命令入队：当客户端处于事务状态时，服务器会根据客户端发来的命令执行不同的操作：

exec、discard、watch、multi 命令会被立即执行
其他命令不会立即执行，而是将命令放入到一个事务队列，然后向客户端返回 QUEUED 回复。
3）事务执行：当一个处于事务状态的客户端向服务器发送 exec 命令时，服务器会遍历事务队列，执行队列中的所有命令，最后将结果全部返回给客户端。

不过 redis 的事务并不推荐在实际中使用，如果要使用事务，推荐使用 Lua 脚本，redis 会保证一个 Lua 脚本里的所有命令的原子性。

 

30、Redis 的 Java 客户端有哪些？官方推荐哪个？
Redis 官网展示的 Java 客户端如下图所示，其中官方推荐的是标星的3个：Jedis、Redisson 和 lettuce。




31、Redis 里面有1亿个 key，其中有 10 个 key 是包含 java，如何将它们全部找出来？
1）keys *java* 命令，该命令性能很好，但是在数据量特别大的时候会有性能问题

2）scan 0 MATCH *java* 命令，基于游标的迭代器，更好的选择

SCAN 命令是一个基于游标的迭代器（cursor based iterator）： SCAN 命令每次被调用之后， 都会向用户返回一个新的游标， 用户在下次迭代时需要使用这个新游标作为 SCAN 命令的游标参数， 以此来延续之前的迭代过程。

当 SCAN 命令的游标参数被设置为 0 时， 服务器将开始一次新的迭代， 而当服务器向用户返回值为 0 的游标时， 表示迭代已结束。

 

32、使用过 Redis 做消息队列么？
Redis 本身提供了一些组件来实现消息队列的功能，但是多多少少都存在一些缺点，相比于市面上成熟的消息队列，例如 Kafka、Rocket MQ 来说并没有优势，因此目前我们并没有使用 Redis 来做消息队列。

关于 Redis 做消息队列的常见方案主要有以下：

1）Redis 5.0 之前可以使用 List（blocking）、Pub/Sub 等来实现轻量级的消息发布订阅功能组件，但是这两种实现方式都有很明显的缺点，两者中相对完善的 Pub/Sub 的主要缺点就是消息无法持久化，如果出现网络断开、Redis 宕机等，消息就会被丢弃。

2）为了解决 Pub/Sub 模式等的缺点，Redis 在 5.0 引入了全新的 Stream，Stream 借鉴了很多 Kafka 的设计思想，有以下几个特点：

提供了消息的持久化和主备复制功能，可以让任何客户端访问任何时刻的数据，并且能记住每一个客户端的访问位置，还能保证消息不丢失。
引入了消费者组的概念，不同组接收到的数据完全一样（前提是条件一样），但是组内的消费者则是竞争关系。
Redis Stream 相比于 pub/sub 已经有很明显的改善，但是相比于 Kafka，其实没有优势，同时存在：尚未经过大量验证、成本较高、不支持分区（partition）、无法支持大规模数据等问题。

 

33、Redis 和 Memcached 的比较
1）数据结构：memcached 支持简单的 key-value 数据结构，而 redis 支持丰富的数据结构：String、List、Set、Hash、SortedSet 等。

2）数据存储：memcached 和 redis 的数据都是全部在内存中。

网上有一种说法 “当物理内存用完时，Redis可以将一些很久没用到的 value 交换到磁盘，同时在内存中清除”，这边指的是 redis 里的虚拟内存（Virtual Memory）功能，该功能在 Redis 2.0 被引入，但是在 Redis 2.4 中被默认关闭，并标记为废弃，而在后续版中被完全移除。

3）持久化：memcached 不支持持久化，redis 支持将数据持久化到磁盘

4）灾难恢复：实例挂掉后，memcached 数据不可恢复，redis 可通过 RDB、AOF 恢复，但是还是会有数据丢失问题

5）事件库：memcached 使用 Libevent 事件库，redis 自己封装了简易事件库 AeEvent

6）过期键删除策略：memcached 使用惰性删除，redis 使用惰性删除+定期删除

7）内存驱逐（淘汰）策略：memcached 主要为 LRU 算法，redis 当前支持8种淘汰策略，见本文第16题

8）性能比较

按“CPU 单核” 维度比较：由于 Redis 只使用单核，而 Memcached 可以使用多核，所以在比较上：在处理小数据时，平均每一个核上 Redis 比 Memcached 性能更高，而在 100k 左右的大数据时， Memcached 性能要高于 Redis。
按“实例”维度进行比较：由于 Memcached 多线程的特性，在 Redis 6.0 之前，通常情况下 Memcached 性能是要高于 Redis 的，同时实例的 CPU 核数越多，Memcached 的性能优势越大。
至于网上说的 redis 的性能比 memcached 快很多，这个说法就离谱。







