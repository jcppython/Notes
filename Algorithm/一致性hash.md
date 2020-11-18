# Consistent Hashing

解决 hash 算法存在 “雪崩问题”

## 雪崩问题
服务发现场景下, 希望同样的请求尽量落到一台机器上, hash算法能够满足, 但是存在 “雪崩” 问题,
如 m 台服务器变更为 n 台时, `hash(id) % m -> hash(id) % n`, 则几乎所有请求的发送目的地都发生变化,
如果是缓存场景, 如果目的地是缓存服务, 所有缓存将失效, 继而对原本被缓存遮挡的数据库或计算服务造成请求风暴, 触发雪崩。

## 一致性hash

- 平衡性 (Balance) : 每个节点被选到的概率是O(1/n)
- 单调性 (Monotonicity) : 当新节点加入时, 不会有请求在老节点间移动, 只会从老节点移动到新节点. 当有节点被删除时, 也不会影响落在别的节点上的请求
- 分散性 (Spread) : 当上游的机器看到不同的下游列表时(在上线时及不稳定的网络中比较常见), 同一个请求尽量映射到少量的节点中
- 负载 (Load) : 当上游的机器看到不同的下游列表的时候, 保证每台下游分到的请求数量尽量一致

## 实现方式

## Locality-aware load balancing

根据下游节点的负载分配流量，快速规避失效的节点，以下游节点的吞吐除以延时作为分流权值。

待阅读: [apache/incubator-brpc]

## 参考
- [一致性哈希算法原理]: https://www.cnblogs.com/lpfuture/p/5796398.html
- [apache/incubator-brpc]: https://github.com/apache/incubator-brpc/blob/master/docs/cn/consistent_hashing.md
- [apache/incubator-brpc]: https://github.com/apache/incubator-brpc/blob/master/docs/cn/lalb.md

