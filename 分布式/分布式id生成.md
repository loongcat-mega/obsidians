id要求:
1. 全局唯一id
2. 趋势递增：mysql数据库使用的innodb存储引擎，使用的聚集索引，使用有序的主键id保证写入的效率
3. 单调自增：保证事务版本号，排序等要求
4. 信息安全：ID递增不规则，防止被恶意爬取数据

# 分布式id生成方案

## UUID

uuid（(universally unique identifier 通用唯一标识码）
包含36个16进制数字，分成5段，"8-4-4-4-12"的36个字符

优点
- 性能高，本地生成，不依赖网络

缺点
- 不易存储
- 信息不安全，基于MAC地址生成的UUID算法可能导致MAC地址泄露
- 无序可能影响性能

## 数据库自增

建立一个数据库作为生成id的数据库：
使用replace into ，如果插入的数据存在的话，会将原数据删除，然后插入一条数据，新插入数据的id作为全局唯一id
并设置好auto_increment_increment 和 auto_increment_offset
每台机器设置不同的初始值，且步长和机器数相等


[分布式数据库自增id](https://code.flickr.net/2010/02/08/ticket-servers-distributed-unique-primary-keys-on-the-cheap/)

缺点
- id没有单调递增的特性，只能趋势递增
- 每次生成id都要读取数据库

## 号段模式

从数据库中获取一个号段范围
```SQL
CREATE TABLE id_generator (
  id int(10) NOT NULL,
  max_id bigint(20) NOT NULL COMMENT '当前最大id',
  step int(20) NOT NULL COMMENT '号段的布长',
  biz_type	int(20) NOT NULL COMMENT '业务类型',
  version int(20) NOT NULL COMMENT '版本号',
  PRIMARY KEY (`id`)
) 
```
等ID都用了，再去数据库获取，然后更改最大值
```SQL
update id_generator set max_id = #{max_id+step}, version = version + 1 where version = # {version} and biz_type = XXX
```
减小了数据库压力


百度：Uidgenerator
美团：leaf

## Redis

incr和incrby自增原子命令，由于单线程，保证了ID唯一和有序。
如果并发量上来之后，就需要集群，又要和传统数据库一样，设置分段和步长

## 雪花算法 snowFlake

twitter开源
以命名空间的方式将64bit分割为多个部分，每个部分代表不同的含义。
从高到低：
- 1bit不用，保证生成的uid是正数
- 41bit时间戳，每个数代表毫秒
- 10bit工作机器id
- 12bit序列号

优点：
- 由于时间戳的特性，id是趋势递增的，并且不依赖第三方系统
缺点
- 依赖机器时钟，如果会退前生成过一些id，回退之后，id有可能重复



## 百度Uidgenerator

[Uidgenerator](https://github.com/baidu/uid-generator/blob/master/README.zh_cn.md)

## 美团 leaf

[mtleaf](https://tech.meituan.com/2017/04/21/mt-leaf.html)
号段模式
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20250417202112.png)
## 滴滴TinyID

[tinyid](https://github.com/didi/tinyid/wiki)

