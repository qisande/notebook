### [官方文档](https://redis.io/docs/latest/develop/get-started/)

### 安装 Redis（docker）

https://cloud.tencent.com/developer/article/1997596

### Redis 的基本概念

> Redis（Remote Dictionary Server）是一个开源的内存中数据结构存储系统，广泛用于缓存、会话管理、实时数据分析等场景。它支持多种数据结构，如字符串（string）、哈希（hash）、列表（list）、集合（set）、有序集合（sorted set）等，并且提供丰富的功能，如事务、发布/订阅、持久化等。

1. **键（Key）**：Redis 中的所有数据都是以键值对的形式存储的，键是字符串类型。
2. **值（Value）**：值可以是多种数据结构，包括字符串、哈希、列表、集合和有序集合等。
3. **数据库（Database）**：Redis 实例包含多个独立的数据库，默认是 16 个，通过索引 0-15 进行访问。

### Redis 基础用法

#### 1. 启动 Redis

在使用 Redis 之前，需要确保 Redis 服务器已经启动。可以通过以下命令启动 Redis 服务器：

```
sh
复制代码
redis-server
```

然后，可以通过 `redis-cli` 命令行客户端连接到 Redis 服务器：

```
sh
复制代码
redis-cli
```

#### 2. 基本命令

##### 字符串（String）

- 设置键值对：

  ```
  sh
  复制代码
  SET key value
  ```

- 获取键的值：

  ```
  sh
  复制代码
  GET key
  ```

- 自增：

  ```
  sh
  复制代码
  INCR key
  ```

##### 哈希（Hash）

- 设置哈希字段：

  ```
  sh
  复制代码
  HSET key field value
  ```

- 获取哈希字段的值：

  ```
  sh
  复制代码
  HGET key field
  ```

- 获取哈希所有字段和值：

  ```
  sh
  复制代码
  HGETALL key
  ```

##### 列表（List）

- 向列表左侧推入元素：

  ```
  sh
  复制代码
  LPUSH key value
  ```

- 向列表右侧推入元素：

  ```
  sh
  复制代码
  RPUSH key value
  ```

- 获取列表中的元素：

  ```
  sh
  复制代码
  LRANGE key start stop
  ```

##### 集合（Set）

- 向集合中添加元素：

  ```
  sh
  复制代码
  SADD key member
  ```

- 获取集合中的所有元素：

  ```
  sh
  复制代码
  SMEMBERS key
  ```

##### 有序集合（Sorted Set）

- 添加带有分数的元素：

  ```
  sh
  复制代码
  ZADD key score member
  ```

- 获取有序集合中的元素：

  ```
  sh
  复制代码
  ZRANGE key start stop [WITHSCORES]
  ```

#### 3. 持久化

Redis 提供了两种持久化方式：RDB（快照）和 AOF（追加日志文件）。

- **RDB（Redis Database Backup）**：定期生成数据快照并保存到磁盘。命令：

  ```
  sh
  复制代码
  SAVE
  ```

- **AOF（Append Only File）**：记录每次写操作并追加到文件末尾。AOF 可以通过以下命令启用：

  ```
  sh
  复制代码
  CONFIG SET appendonly yes
  ```

#### 4. 发布/订阅（Pub/Sub）

Redis 支持发布/订阅消息模式，允许消息的发送方（发布者）发布消息，接收方（订阅者）接收消息。

- 订阅频道：

  ```
  sh
  复制代码
  SUBSCRIBE channel
  ```

- 向频道发布消息：

  ```
  sh
  复制代码
  PUBLISH channel message
  ```

### Java 使用 Redis

可以通过 Spring Data Redis 或 Jedis 等库在 Java 应用程序中使用 Redis。以下是一个使用 Jedis 的简单示例：

1. 添加依赖：

   ```
   xml复制代码<dependency>
       <groupId>redis.clients</groupId>
       <artifactId>jedis</artifactId>
       <version>4.0.1</version>
   </dependency>
   ```

2. 使用 Jedis 进行基本操作：

   ```
   java复制代码import redis.clients.jedis.Jedis;
   
   public class RedisExample {
       public static void main(String[] args) {
           // 连接到 Redis 服务器
           Jedis jedis = new Jedis("localhost");
   
           // 设置键值对
           jedis.set("name", "Alice");
   
           // 获取键的值
           String value = jedis.get("name");
           System.out.println("Value: " + value);
   
           // 关闭连接
           jedis.close();
       }
   }
   ```

### docker 使用