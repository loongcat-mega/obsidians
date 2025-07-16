## DEL
用于删除给定的一个或多个 `key` 。
不存在的 `key` 会被忽略。
```bash
DEL key [key ...]
```
基本语法：
```bash 
redis 127.0.0.1:6379> DEL KEY_NAME
```
返回值：
整数：被删除 key 的数量

**时间复杂度:** O(N)，其中N是将被移除的key的数量。当要删除的键包含字符串以外的值时，该键的单个复杂度为O(M)，其中M是列表、集合、有序集合或哈希中的元素数。删除保存字符串值的单个键是O(1)。

## DUMP
用于序列化给定 `key` ，并返回被序列化的值，使用 `restore` 命令可以将`DUMP` 的结果反序列化回 Redis 。
```
redis 127.0.0.1:6379> DUMP KEY_NAME
```
返回值：
多行字符串：如果 key 不存在，那么返回 nil 。 否则，返回序列化之后的值。

## EXISTS
命令用于检查给定 `key` 是否存在。
从 Redis 3.0.3 起可以一次检查多个 key 是否存在。这种情况下，返回待检查 key 中存在的 key 的个数。检查单个 key 返回 1 或 0 。
注意：如果相同的 key 在参数列表中出现了多次，它会被计算多次。所以，如果`somekey`存在, `EXISTS somekey somekey` 命令返回 2。
```
redis 127.0.0.1:6379> EXISTS KEY_NAME
```
返回值：
整数：0-key 不存在 ，1-key 存在

## EXPIRE

`Expire` 命令设置 `key` 的过期时间（seconds）。 设置的时间过期后，key 会被自动删除。带有超时时间的 key 通常被称为易失的(_volatile_)。
超时时间只能使用删除 key 或者覆盖 key 的命令清除，对于修改 key 中存储的值，而不是用新值替换旧值的命令，不会修改超时时间例如，自增 key 中存储的值的 INCR , 向list中新增一个值 LPUSH, 或者修改 hash 域的值 HSET ，这些都不会修改 key 的过期时间。

通过使用 PERSIST 命令把 key 改回持久的(persistent) key，这样 key 的过期时间也可以被清除。
```
redis 127.0.0.1:6379> EXPIRE key seconds
```
一般来说 Redis 中创建 key 的是不带生存时间，如果你不使用类似 DEL 命令明确删除它，这个key将会一直存在。


### 如何淘汰过期 Key

两种淘汰方式：被动和主动
被动过期：用户访问某个key 时，key 被发现过期。但是该方式对于那些永远也不会被访问到的 key 并没有效果
主动过期：周期性主动随机检查一部分被设置生存时间的 key，那些过期的 key 会被删除，redis 每s 执行 10 次如下操作：
1. 从带有生存时间的 key 集合中随机选 20 个进行检查
2. 删除所有过期的 key
3. 如 20 里面有超过 25% 的 key 过期，立即执行步骤 1
这是一个狭义概率算法，我们假设我们选出来的样本 key 代表整个 key 空间，我们继续过期检查直到过期 key 的比例降到 25% 以下

## EXPIREAT
与 EXPIRE 有相同的作用和语义, 不同的是 EXPIREAT 使用绝对 Unix 时间戳 (自1970年1月1日以来的秒数)代替表示过期时间的秒数。使用过去的时间戳将会立即删除该 key。
EXPIREAT 引入的目的是为了把 AOF 持久化模式的相对时间转换为绝对时间。当然，也可以直接指明某个 key 在未来某个时间过期。
```
redis 127.0.0.1:6379> Expireat KEY_NAME TIME_IN_UNIX_TIMESTAMP
```

## KEYS

用于查找所有匹配给定模式 pattern 的 key 。
- `h?llo` 匹配 `hello`, `hallo` 和 `hxllo`
- `h*llo` 匹配 `hllo` 和 `heeeello`
- `h[ae]llo` 匹配 `hello` and `hallo,` 不匹配 `hillo`
- `h[^e]llo` 匹配 `hallo`, `hbllo`, ... 不匹配 `hello`
- `h[a-b]llo` 匹配 `hallo` 和 `hbllo`
使用 `\` 转义你想匹配的特殊字符。
```
redis 127.0.0.1:6379> KEYS PATTERN
```
返回值：
数组：以数组形式返回匹配模式 pattern 的 key 列表


## MIGRATE

将 key 原子性地从当前实例传送到目标实例的指定数据库上，一旦传送成功， key 会出现在目标实例上，而当前实例上的 key 会被删除。
它在当前实例对给定 key 执行 DUMP 命令 ，将它序列化，然后传送到目标实例，目标实例再使用 RESTORE 对数据进行反序列化，并将反序列化所得的数据添加到数据库中；当前实例就像目标实例的客户端那样，只要看到 RESTORE 命令返回 OK ，它就会调用 DEL 删除自己数据库上的 key 。

## MOVE
用于将当前数据库的 key 移动到选定的数据库 db 当中
如果 `key` 在目标数据库中已存在，或者 `key` 在源数据库中不存，则`key` 不会被移动。
```
redis 127.0.0.1:6379> MOVE KEY_NAME DESTINATION_DATABASE
```

返回值：
整数：1-移动成功，0-没有移动


## OBJECT
命令允许从内部察看给定 key 的 Redis 对象， 它通常用在除错(debugging)或者了解为了节省空间而对 key 使用特殊编码的情况。 当将Redis用作缓存程序时，你也可以通过 OBJECT 命令中的信息，决定 key 的驱逐策略(eviction policies)。

OBJECT 命令有多个子命令：
- `OBJECT REFCOUNT <key>` 返回给定 `key` 引用所储存的值的次数。此命令主要用于除错。
- `OBJECT ENCODING <key>` 返回给定 `key` 锁储存的值所使用的内部表示(representation)。
- `OBJECT IDLETIME <key>` 返回给定 `key` 自储存以来的空闲时间(idle， 没有被读取也没有被写入)，以秒为单位。

对象可以以多种方式编码：
- 字符串可以被编码为 `raw` (一般字符串)或 `int` (为了节约内存，Redis 会将字符串表示的 64 位有符号整数编码为整数来进行储存）。
- 列表可以被编码为 `ziplist` 或 `linkedlist` 。 `ziplist` 是为节约大小较小的列表空间而作的特殊表示。
- 集合可以被编码为 `intset` 或者 `hashtable` 。 `intset` 是只储存数字的小集合的特殊表示。
- 哈希表可以编码为 `zipmap` 或者 `hashtable` 。 `zipmap` 是小哈希表的特殊表示。
- 有序集合可以被编码为 `ziplist` 或者 `skiplist` 格式。 `ziplist` 用于表示小的有序集合，而 `skiplist` 则用于表示任何大小的有序集合。


返回值：
`REFCOUNT` 和 `IDLETIME` 返回数字。 `ENCODING` 返回相应的编码类型。
- Subcommands `refcount` and `idletime` return integers.
- Subcommand `encoding` returns a bulk reply.

## PERSISTI
命令用于删除给定 key 的过期时间，使得 key 永不过期。
```
redis 127.0.0.1:6379> PERSIST KEY_NAME
```

返回值：
整数：1-过期时间移除成功；0-key 不存在或者 key 没有设置过期时间


## PEXPIRE
跟 EXPIRE 基本一样，只是过期时间单位是毫秒。
```
PEXPIRE key milliseconds
```
## PEXPIREAT
命令用于设置 key 的过期时间，时间的格式是uinx时间戳并精确到毫秒。
```
redis 127.0.0.1:6379> PEXPIREAT KEY_NAME TIME_IN_MILLISECONDS_IN_UNIX_TIMESTAMP
```
## TTL
以秒为单位返回 key 的剩余过期时间。用户客户端检查 key 还可以存在多久。


 ## PTTL
以毫秒为单位返回 key 的剩余过期时间。
```
redis 127.0.0.1:6379> PTTL KEY_NAME
```
返回值：
整数 ：key 不存在：-2；key 存在但没有设置生存时间：-1；否则返回以毫秒为单位的剩余生存时间

## RANDOMKEY

从当前数据库中随机返回一个 key 。
```
redis 127.0.0.1:6379> RANDOMKEY 
```
返回值：
多行字符串：数据库为空返回 nil ，否则返回 key

## RENAME

用于修改 key 的名字为 `newkey` 。若key 不存在返回错误
在集群模式下，key 和newkey 需要在同一个 hash slot。key 和newkey有相同的 hash tag 才能重命名。
如果 newkey 存在则会被覆盖，此种情况隐式执行了 DEL 操作，所以如果要删除的key的值很大会有一定的延时，即使RENAME 本身是常量时间复杂度的操作。
在集群模式下，key 和newkey 需要在同一个 hash slot。key 和newkey有相同的 hash tag 才能重命名。

```
RENAME key new_key
```

## RENAMENX

用于在新的 key 不存在时修改 key 的名称 。若 key 不存在返回错误。
```
redis 127.0.0.1:6379> RENAMENX OLD_KEY_NAME NEW_KEY_NAME
```
返回值：
整数：1-成功；0-newkey 已经存在

## SCAN
Redis SCAN 命令及其相关命令 SSCAN、HSCAN、 ZSCAN 命令都是用于增量遍历集合中的元素。

SCAN 用于遍历当前数据库中的键。
SSCAN 用于遍历集合键中的元素。
HSCAN 用于遍历哈希键中的键值对。
ZSCAN 用于遍历有序集合中的元素（包括元素成员和元素分值）。

```
SCAN cursor [MATCH pattern] [COUNT count]
```
- cursor - 游标。
- pattern - 匹配的模式。
- count - 指定从数据集里返回多少元素，默认值为 10 。

SCAN 返回一个包含两个元素的数组， 第一个元素是用于进行下一次遍历的新游标， 而第二个元素则是一个数组， 这个数组中包含了所有被遍历的元素。当 SCAN 命令的游标参数被设置为 0 时， 服务器将开始一次新的遍历，而当服务器向用户返回值为 0 的游标时， 表示遍历已结束。


## SORT
```
SORT key [BY pattern] [LIMIT offset count] [GET pattern [GET pattern ...]] [ASC|DESC] [ALPHA] [STORE destination]
```


## TOUCH

修改指定 key 的 最后访问时间。忽略不存在的 key。
```
TOUCH key [key...]
```
返回值：
整数：被更新的 key个数

## TYPE
以字符串的形式返回存储在 `key` 中的值的类型
可返回的类型是: `string`, `list`, `set`, `zset`,`hash` 和 `stream`。

```
type key
```

## UNLINK

UNLINK 命令跟 DEL 命令十分相似：用于删除指定的 key 。就像 DEL 一样，如果 key 不存在，则将其忽略。但是，该命令会执行命令之外的线程中执行实际的内存回收，因此它不是阻塞，而 DEL 是阻塞的。这就是命令名称的来源：UNLINK 命令只是将键与键空间断开连接。实际的删除将稍后异步进行。

```
redis 127.0.0.1:6379> UNLINK key_name 
```

返回值：
整数：断开连接的 key 的数量

## WAIT

WAIT 命令用来阻塞当前客户端，直到所有先前的写入命令成功传输并且至少由指定数量的从节点复制完成。如果执行超过超时时间（以毫秒为单位），则即使尚未完成指定数量的从结点复制，该命令也会返回。
WAIT 命令总是返回在 WAIT 命令之前发送的写入命令被复制到的从结点数量。

几点说明:
1. 当 WAIT 返回时，在当前连接的上下文中发送的所有先前的写入命令被保证由 WAIT 返回的数量的从结点接收。
2. 如果该命令是作为 MULTI 事务的一部分发送的，则该命令不会阻塞，而是仅返回 ASAP 确认上一个 WAIT 命令写入从结点的数量。
3. 超时时间 0 意味着永久阻止。
4. 由于 WAIT 返回在失败和成功时都达到的从结点数量，客户端应检查返回的值是否等于或大于它所要求的复制数量。
```
redis 127.0.0.1:6379> WAIT numreplicas timeout
```