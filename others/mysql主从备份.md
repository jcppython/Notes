## 1. 修改主从配置

```sh
log-bin=mysql-bin # [必须]启用二进制日志
server-id=222     # [必须]服务器唯一ID，默认是1，一般取IP最后一段
# [可选] [解释部分配置项]
# 不加binlog-do-db和binlog_ignore_db，那就表示备份全部数据库
binlog-do-db=wordpress # 表示只备份wordpress数据库
binlog_ignore_db=mysql # 表示忽略备份mysql
```

注：主从server-id是不同的~



## 2. 在主服务器master上建立帐户并授权slave

在主服务器新建一个用户赋予“REPLICATION SLAVE”的权限。你不需要再赋予其它的权限。在下面的命令，把X.X.X.X替换为从服务器的IP.

```mysql
mysql>CREATE USER 'user'@'X.X.X.X' IDENTIFIED BY 'password';
mysql>GRANT REPLICATION SLAVE ON *.* TO 'user'@'X.X.X.X' IDENTIFIED BY 'password';
```



## 3. 启动从服务器复制功能前，保证主从一致

-   主数据库数据全量导入从服务器

```sh
# 只导入要同步的数据库就可以  (eg: binlog-do-db=wordpress 只导入wordpress即可)
mysqldump -u root -p123456 --all-databases  --lock-tables=false  -- > /tmp/all.sql
mysql -u root -p123456 < /root/all.sql
```



## 4. 从服务器slave的其他配置
```mysql
# 注意 执行完此步骤后不要再操作主服务器MYSQL，防止主服务器状态值变化
CHANGE MASTER TO MASTER_HOST='xx.xx.xx.xx', MASTER_USER='repl', MASTER_PASSWORD='123456', MASTER_LOG_FILE='mysql-bin.000048', MASTER_LOG_POS=4039827;

# 上述几个参数可以通过在主服务器执行下列命令查看
mysql>show master status;

# 启动从服务器复制功能
mysql>start slave;
```



-   如何防止主服务器被操作
```mysql
# 防止主服务器状态变化，锁定写
mysql>FLUSH TABLES WITH READ LOCK;
# 解锁
mysql>UNLOCK TABLES
```



## 5. 检查从服务器复制功能状态

```mysql
# 注：Slave_IO及Slave_SQL进程必须正常运行，即YES状态，否则都是错误的状态(如：其中一个NO均属错误)
mysql> show slave status\G;
```



## 6. 其他

编写一shell脚本，用nagios监控slave的两个yes（Slave_IO及Slave_SQL进程），如发现只有一个或零个yes，就表明主从有问题了，发短信警报吧