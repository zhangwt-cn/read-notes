## InnoDB

### [Buffer Pool](https://dev.mysql.com/doc/refman/8.0/en/innodb-buffer-pool.html)
1. 定义
Buffer Pool 是InnoDB是为缓存数据页在内存中开辟的空间
2. 如何实现的
Buffer Pool 使用链表缓存数据页，整个链表划分为 new（5/8） 和 old（3/8） 两个链表，使用LRU算法管理这两个链表，最新读取的数据页插入到old链表顶部，随着访问次数增加会从old链表移动到new链表顶部。
3. 为什么这样设计
- 为什么使用LRU
  内存热点数据缓存，增加并发性能
- 为什么使用链表
- 为什么要划分new 和 old
  为了使数据更快淘汰出内存，加载数据页到 Buffer Pool 中，在一些情况下会加载大量数据，为了使非热点数据快速淘汰，将这些数据都加载到 old 列表头部，等到访问次数增加再从old头部移动到new列表。
4. 优化配置
- 增加 Buffer Pool 大小
- 划分多个 Buffer Pool 增加并发访问
- 配置预读
- 配置 Buffer Pool 状态保留，减少重启时间
- 配置刷新率
