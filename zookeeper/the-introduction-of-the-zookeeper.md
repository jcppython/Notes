# zookeeper
> zk = 文件系统 + 监听通知机制

分布式服务框架, Apache Hadoop 的一个子项目
解决分布式应用中的数据管理问题, 如：统一命名服务、状态同步服务、集群管理、分布式应用配置项的管理等

以 Fast Paxos 算法为基础，实现同步服务、配置维护和命名服务等分布式应用

## 文件系统

四种类型的znode, znode 是有版本的

### PERSISTENT(持久化目录节点)

客户端与zookeeper断开连接后，该节点依旧存在

### PERSISTENT_SEQUENTIAL(持久化顺序编号目录节点)

客户端与zookeeper断开连接后，该节点依旧存在，只是zookeeper给该节点名称进行顺序编号

### EPHEMERAL(临时目录节点)

客户端与zookeeper断开连接后，该节点被删除
不能有子节点目录

### EPHEMERAL_SEQUENTIAL(临时顺序编号目录节点)

客户端与zookeeper断开连接后，该节点被删除，只是Zookeeper给该节点名称进行顺序编号

## 监听通知机制

客户端注册监听它关心的目录节点，当目录节点发生变化（数据改变、被删除、子目录节点增加删除）时，zookeeper会通知客户端

watch有3个关键点
1. watch是一次性的, 当触发过一次之后, 除非重新设置, 新的数据变化不会通知客户端
2. 通知是将数据 “发生了改变” 通知客户端，而不是将 “改变后的内容” 通知客户端
3. watch 由读取操作, 如 getData() 设置, 由修改操作触发

watch保留在ZK服务器上, 当客户端断线重连后, 与它的相关的watch都会自动重新注册, 对客户端来说是透明的.
特定情况下, watch事件会被错过: 客户端B设置了关于节点A存在性watch, 但B断线了, 在B断线过程中节点A被创建又被删除; 之后, B再连线后不知道A节点曾经被创建过, 但这并不影响最终的一致性

ZK的watch机制保证以下几点：

1. 事件的通知顺序与事件触发顺序一致。
2. 客户端先接收到更新事件，才能查到更新后的数据
3. 事件触发的顺序与ZK服务器上数据变化的顺序一致

## 会话

1. 客户端保存了所有ZK服务器列表, 并在初始化连接时随机选择服务器.
2. 当客户端向ZK发起连接请求时, ZK会创建一个会话（Session）, 并产生一个64位数值的会话ID.
3. 如果客户端连接其他ZK服务器, 则会把这个会话ID作为握手的一个参数. 为了保证数据传输安全, 与会话ID相关联的一个密码会发送给客户端。会话ID与密码的关联关系, 集群中的任何一台ZK服务器都可以验证。

ZK的会话通过客户端发送的请求来维持, 如果客户端一段时间没有发送请求, 则客户端会发送PING请求来保持会话. 这个心跳机制也保证了服务器端了解客户端的存活状态。

## 数据访问

1. 数据访问都是原子的

- create：创建一个znode（必须要有父节点）
- delete：删除一个znode（该znode不能有任何子节点）
- exists：测试一个znode是否存在并且查询他的元数据
- getACL，setACL：获取/设置一个znode的ACL（ACL代表该节点的访问权限）
- getChildren：获取一个znode的子节点名字列表
- getData，setData：读/写一个znode的数据
- sync：同步follower与ZK的数据

[zookeeper]: http://zookeeper.apache.org/
[分布式系统服务ZooKeeper的学习历程]: https://github.com/llohellohe/zookeeper
[ZooKeeper典型应用场景一览]: https://www.cnblogs.com/tommyli/p/3766189.html
[分布式服务框架 Zookeeper —— 管理分布式环境中的数据]: https://developer.ibm.com/zh/articles/os-cn-zookeeper/

