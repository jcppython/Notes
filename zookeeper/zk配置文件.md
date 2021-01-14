# zk配置文件
ZooKeeper集群进行配置的时候，它的配置文档是完全相同的

# 1. 必须配置
1. client: 监听客户端连接的端口
2. tickTime: 基本事件单元，这个时间是作为ZooKeeper服务器之间或客户端与服务器之间维持心跳的时间间隔，每隔tickTime时间就会发送一个心跳；最小的session过期时间为2倍tickTime
3. dataDir: 存储内存中数据库快照的位置，如果不设置参数，更新事务的日志将被存储到默认位置

```
注意
应该谨慎的选择日志存放的位置，使用专用的日志存储设备能够大大提高系统的性能，如果将日志存储在比较繁忙的存储设备上，那么将会很大程度上影响系统性能
```

# 2. 高级配置
1. dataLogdDir:

把事务日志写入"dataLogDir"，而不是"dataDir"，这将允许使用一个专用的日志设备，帮助我们避免日志和快照的竞争，配置如下：
```
# the directory where the snapshot is stored
dataDir=/usr/local/zk/data
```

2. maxClientCnxns

限制连接到ZooKeeper的客户端数量，并限制并发连接的数量，通过IP来区分不同的客户端
可以阻止某些类别的Dos攻击，将他设置为零(默认值)会取消对并发连接的限制

```
# set maxClientCnxns
maxClientCnxns=1
启动ZooKeeper之后，首先用一个客户端连接到ZooKeeper服务器上。之后如果有第二个客户端尝试对ZooKeeper进行连接，或者有某些隐式的对客户端的连接操作，将会触发ZooKeeper的上述配置
```


3. minSessionTimeout和maxSessionTimeout

即最小的会话超时和最大的会话超时时间
默认值，minSession=2*tickTime；maxSession=20*tickTime

# 3. 集群配置
1. initLimit

此配置表示，允许follower（相对于Leaderer言的“客户端”）连接并同步到Leader的初始化连接时间，以tickTime为单位。当初始化连接时间超过该值，则表示连接失败

2. syncLimit

此配置项表示Leader与Follower之间发送消息时，请求和应答时间长度。如果Follower在设置时间内不能与Leader通信，那么此Follower将会被丢弃。

3. server.A=B:C:D

A: 其中A是一个数字，表示这个是服务器的编号
B: 是这个服务器的IP地址
C: Leader选举的端口
D: ZooKeeper服务器之间的通信端口

4. myid和zoo.cfg

除了修改zoo.cfg配置文件，集群模式下还要配置一个文件myid，这个文件在dataDir目录下，这个文件里面就有一个数据就是A的值，ZooKeeper启动时会读取这个文件，拿到里面的数据与zoo.cfg里面的配置信息比较从而判断到底是那个Server


# 参考
https://www.cnblogs.com/EasonJim/p/7482961.html

