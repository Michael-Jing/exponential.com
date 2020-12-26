+++
+++
# disk managerment
## install system-storage-manager

`yum install system-storage-manager`

## list disk information
 `ssm list`

 system is using xfs file system


# install scylladb
## install epel-repo and wget tool
`yum install epel-release wget`
## add scylladb repo
`sudo wget -O /etc/yum.repos.d/scylla.repo http://downloads.scylladb.com/rpm/centos/scylla-1.6.repo`
## install scylladb
`yum install scylla`

## configure scylla
cluster_name : sophon scylla cluster
seeds "172.22.2.6"
listen_address: address of server
rpc_address: address of server
api_address: address of server

## run scylla_setup script to tune system settings
`scylla_setup`

## reboot host machine to make changes in boot options effective

## start scylla-server
`systemctl start scylla-server`

## write data to scylla server

## nodetool 学习
cfhistograms
nodetool cfhistograms <keyspacename> <column family name>

cfstats
nodetool tfstats <keyspacename>.<column family name>
给出每一个列族的详细信息

cleanup - Triggers the immediate cleanup of keys no longer belonging to a node ?

clearsnapshot - removess snapshots
compactionhistory -
compactionstas - displays number of pending tasks

compact - Force a (major) compaction on one or more culumn families

decommission - Deactivate a selected node by streaming his data to other nodes in the cluster

disablegossip - Disable gossip (effectively marking the node down)

drain - Drain the node (stop accepting writes and flush all column families)

flush - Flush one or more column families

removenode

repair

## decommission和恢复
`nodetool decommission`会将当前node上的数据传输到其他node上保存， 并将当前node从集群中退出。
退出之后将当前node重新加入到集群中的方法是：
将/var/lib/scylla/commitlog 和/var/lib/scylla/data下面的所有数据删除， 然后重新启动scylla服务
，启动服务的命令如下：
systemctl restart scylla-server

## decommission后多长时间数据能够平衡
decommission之后几分钟之内就会看到另外一个node上的数据显著增加。

## 删除一个node上面的数据
使用`systemctl stop scylla-server`
删除commitlog 和data中的数据
使用`systemctl start scylla-server`重新启动是无法启动， 会看到日志中有如下信息：[shard 0] seastar
- Exiting on unhandled exception: std::runtime_error (A node with address 172.22.2.7)
already exists, cancelling join. Use replace_address if you want to replace this node.)

如何解决这个问题：
在配置文件/etc/scylla/scylla.yaml中增加replace_address: 172.22.2.7(替换成需要的IP地址)

这里注意到的一个问题是是scylla-server起来之后数据量不会自动平衡， 因为用`nodetool status`可以看到被
删除数据的node上面的数据量很少


## 在不停止服务的情况下删除数据

在小数据量下， 在不停止service的情况下直接删除commitlog和data目录中的数据以后， 用
`nodetool status`检查集群的状态还是和之前没有不同
但在运行`nodetool cleanup`的时候， 事情开始变得不同。

*在decommission一个node， 或者删除一个node上的数据这种操作之后， 其他node上的数据量会发生变化，
但是并没有观察到数据量的完全平衡， 数据量之间的差别较大*

在经过一系列的操作之后， 4个node上面的数据量分别是112MB， 33MB， 92MB， 34MB， 其中数据量33
和34MB的node是经历过数据被删除的node。

## 杀死进程
scylla的主进程很难杀死， 因为进程在退出前会启动新的进程，
但是杀死进程对与系统正常运行还是有影响， 在测试中进程被杀死时， 向集群写入数据的客户端程序发生了退出。
，即使使用了kill -9也是这样。

## 关于数据量平衡
数据量平衡似乎只是一个不怎么强的平衡， 比如现在在向集群写入了一些数据之后的状态是数据量
8.65GB， 5.18GB 8.81GB， 8.52GB

## 中数据量平衡试验
1. decommission
decommision前的status：
2.11GB
2.16GB
1.88GB
1.93GB

decommission后的status
3.06GB
3.07GB
2.67GB
在较短的时间内达到了某种成都上的平衡， 平衡速度在1分钟左右

node 重新join之后的数据量， 平衡速度在1分钟左右（相当与磁盘可读的情况）
3.06GB
3.07GB
2.67GB
1.48GB

nodetool cleanup 之后
2.2GB
2.53GB
2GB
1.

decommission 之后删除数据， 然后重新启动服务（这里重启服务大概需要1分钟的时间）， 在重新启动之后， 再过约1分钟的时间完全join
