# 数据更新的持久化方案
update 和 delete 时，更新写到页中，此时这个页就是脏页。

### 方案一，脏页立即写回
页一旦出现变化，就写回磁盘。
#### 问题
1. 若hot 数据集中在某几个页中，页需要不停地写回磁盘，会导致性能差。
2. 写回磁盘时发生宕机，导致内存数据丢失

### 方案二，Write Ahead Log（先写日志，后更新缓存页）
事务提交时，先写 redo log，再更新缓存中的页。


# Write Ahead Log有什么问题
> 可以用 redo log 恢复数据库

1. 日志文件无限增大？
2. 日志文件太大，恢复数据库会很耗时？

# checkpoint 技术
>按规则将缓存脏页刷新到磁盘，刷盘后 log 就无效，因此能保证 log 文件不会无限增大

checkpoint 技术的核心：将缓冲池中的脏页刷回到磁盘。

### Sharp Checkpoint（所有脏页刷回）
发生在数据库关闭时，将所有脏页都刷回磁盘。这是默认工作方式。

问题：若在运行时使用，所有脏页刷回，量太大，STW，导致可用性降低

### Fuzzy Checkpoint（部分脏页刷回）




