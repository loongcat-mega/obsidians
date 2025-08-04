## ZADD
向有序集合中添加一个或者多个成员，或者更新已存在成员的分数
```shell
ZADD key [NX|XX] [GT|LT] [CH] [INCR] score member [score member ...]
```
如果某个 `member` 已经是有序集的成员，那么更新这个 `member` 的 `score` 值，并通过重新插入这个 `member` 元素，来保证该 `member` 在正确的位置上。
### ZADD 参数

ZADD 支持参数，参数位于 key 名字和第一个 score 参数之间:

- **XX**: 仅更新存在的成员，不添加新成员。
- **NX**: 不更新存在的成员。只添加新成员。
- **LT**: 更新新的分值比当前分值小的成员，不存在则新增。
- **GT**: 更新新的分值比当前分值大的成员，不存在则新增。
- **CH**: 返回变更成员的数量。变更的成员是指 **新增成员** 和 **score值更新**的成员，命令指明的和之前score值相同的成员不计在内。 注意: 在通常情况下，ZADD返回值只计算新添加成员的数量。
- **INCR**:ZADD 使用该参数与 ZINCRBY 功能一样。一次只能操作一个score-element对。
注意: GT, LT 和 NX 三者互斥不能同时使用。
## ZCARD
获取有序集合的成员数

## ZCOUNT
计算在有序集合中指定区间分数的成员数

## ZINCRBY
有序集合中对指定成员的分数加上增量 increment


## ZRANGE
