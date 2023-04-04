# Redis 数据结构

> 原文：<https://blog.devgenius.io/redis-data-structures-de5a2eb7211f?source=collection_archive---------5----------------------->

![](img/dce38991d2233b778022d620721ee84a.png)

在本系列的最后一部分，我们稍微熟悉了一下 Redis 是什么。Redis 为我们提供了五种不同的结构，我们将在这一部分逐一探讨它们。
我们的 Redis 服务器已经启动并运行(参考之前关于如何设置服务器的帖子)，我们将使用 Redis CLI 进行实践，然后我们将转移到 python 客户端并构建一个小项目。
在开始讨论数据结构之前，需要注意的重要一点是 Redis 数据库中的每个条目都是{key:value}形式，其中的值可以是我们将要讨论的五种数据结构中的任何一种。Key 必须是字符串格式的，但是对于 value，Redis 给了我们一些自由，不像它的一些对等物，比如 memcache，它把值限制为字符串类型。我们开始吧。

# 用线串

字符串不需要介绍，如果您曾经使用过任何编程语言，那么字符串对您来说一定很常见，在 Redis 中，字符串也不例外。在进入 redis-cli 世界时，检查服务器是否启动并提供服务。

```
guest-c41jf8@ubuntu:~$ redis-cli
127.0.0.1:6379>
127.0.0.1:6379> PING
PONG
```

看起来我们准备好了。

```
127.0.0.1:6379> set hello world
OK
127.0.0.1:6379> get hello
“world”
127.0.0.1:6379> del hello
(integer) 1
127.0.0.1:6379> get hello
(nil)
```

我们在上面的命令中做了什么？首先，我们创建了一个值为“world”的键“hello ”,返回一个 OK 输出来表示创建成功。在使用 get 函数查询键“hello”时，我们返回了相应键的值，最后我们删除了该键，在再次查询键“hello”时，我们可以看到这次返回了 nil，表示不存在这样的键值对。

```
127.0.0.1:6379> append hello is 
(integer) 7
127.0.0.1:6379> get hello
“worldis”127.0.0.1:6379> set counter 1
OK
127.0.0.1:6379> incr counter
(integer) 2
127.0.0.1:6379> get counter
“2”
127.0.0.1:6379> incr counter
(integer) 3
127.0.0.1:6379> get counter
“3”
127.0.0.1:6379> decr counter
(integer) 2127.0.0.1:6379> incrby counter 10
(integer) 12
127.0.0.1:6379> decrby counter 2
(integer) 10
127.0.0.1:6379> incr hello
(error) ERR value is not an integer or out of range
```

通过分解上面的命令，我们得到了一个 append 函数，在这个函数中，我们可以将一些字符串追加到一个键的现有值中。我将“is”追加到键的“world”值中；“你好”，给了我们一个结果“世界是”的后话。Redis 还为我们提供了不同的方法来处理整数，在这里我们可以增加和减少单位数量或指定数量的值。(您注意到了吗，我们在调用这些函数时得到了输出结果？).正如所料，这些函数只有在值是整数类型时才起作用，否则我们将会受到一些错误的欢迎。我们已经探索了 string 的一些基本命令，这里提供了每个用例的完整列表。https://redis.io/commands/#string

# 列表

Aaa，好的老名单。让我们把手弄脏，好吗？

```
127.0.0.1:6379> Lpush movies arrival
(integer) 1
127.0.0.1:6379> rpush movies titanic
(integer) 2
127.0.0.1:6379> lpush movies ironman
(integer) 3
127.0.0.1:6379> lrange movies 0 -1
1) “ironman”
2) “arrival”
3) “titanic”
127.0.0.1:6379> lindex movies 1
“arrival”
127.0.0.1:6379> lpop movies
“ironman”
127.0.0.1:6379> lrange movies 0 -1
1) “arrival”
2) “titanic”
127.0.0.1:6379> rpop movies
“titanic”
127.0.0.1:6379> lrange movies 0 -1
1) “arrival”
```

Redis 中的列表实际上是一个双面列表，我们可以从右边和左边输入元素。这里，我们创建了一个“电影”列表，并输入了几个元素。使用“lrange”我们得到列表中的所有元素，其中末尾的索引表示开始和结束位置。与列表函数类似，我们可以使用 lindex 获取特定索引处的元素，并从两个元素中弹出元素。请注意，我们在执行这些命令时会得到不同的输出，因为它们会对我们的用例有所帮助。

# 设置

集合给了我们列表所不能给的东西，不像列表，集合中的所有元素都是唯一的，但是集合是无序的，因此我们不能像在列表中那样从两端推入和弹出，我们可以简单地添加和删除元素，并保证元素是唯一的。

```
127.0.0.1:6379> sadd fruits melon
(integer) 1
127.0.0.1:6379> sadd fruits melon
(integer) 0
127.0.0.1:6379> sadd fruits grape
(integer) 1
127.0.0.1:6379> srem fruits grape
(integer) 1
127.0.0.1:6379> srem fruits grape
(integer) 0
127.0.0.1:6379> smembers fruits
1) “melon”
```

和往常一样，要注意输出，如果我们添加或删除一个已经添加或不存在的元素，我们得到的输出为 0，否则为 1。我们提供了一些其他选项，如并集、交集等，来处理两个或多个集合。一定要自己动手在完整的命令列表中查看它们。

# 混杂

哈希类似于 Python 中的字典。您可以将值作为键/值对存储在键中。比方说，如果你想为一个产品存储数据，让我们拿一顶帽子，那么散列将是完美的选择。我们来补充一下帽子的产品特性，好吗？

```
127.0.0.1:6379> hset hat:1 id 34234
(integer) 1
127.0.0.1:6379> hset hat:1 color blue
(integer) 1
127.0.0.1:6379> hset hat:1 quantity 100
(integer) 1
127.0.0.1:6379> hgetall hat:1
1) “id”
2) “34234”
3) “color”
4) “blue”
5) “quantity”
6) “100”
127.0.0.1:6379> hdel hat:1 color
(integer) 1
127.0.0.1:6379> hgetall hat:1
1) “id”
2) “34234”
3) “quantity”
4) “100”
```

所以我们为 hat 'hat:1 '创建了一个键，它最初有子项、id、quanity 和 color。我们可以用不同的钥匙创建多个帽子，并创建关于商店出售的不同类型帽子的信息。有意思吧！

# ZSETS

最后一个数据结构，也是一个新的数据结构，是 Redis 特有的。与哈希类似，zset 由键/值类型结构组成，但 zset 中的值通常被称为 score，并且仅限于浮点类型。我们可以选择按排序顺序获取值。

```
127.0.0.1:6379> zadd hats 23 id
(integer) 1
127.0.0.1:6379> zadd hats 234 id:2
(integer) 1
127.0.0.1:6379> zrange hats 0 1
1) “id”
2) “1d:2”
127.0.0.1:6379> zrange hats 0 1 withscores
1) “id”
2) “23”
3) “1d:2”
4) “234”
```

我们创建了两个子键:id 和 id:2，分数分别为 23 和 234。最后我们得到一个排序后的分数输出。如果我们试图添加一个重复的键或值，我们将不能。你自己试试。

我希望这让您对 Redis 提供的数据结构有所了解。在下一部分中，我们将从安装 Redis 的 python 客户端开始，并构建一个小项目。