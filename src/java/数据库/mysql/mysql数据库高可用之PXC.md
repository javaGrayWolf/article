# mysql高可用之PXC

>*就算再想哭，也要微笑的说一句：你大爷的！*

### 简单介绍：

[PXC](https://www.percona.com/doc/percona-xtradb-cluster/LATEST/index.html)（Percona Xtradb Cluster）的实现是Percona公司开发的，在原mysql代码上通过Galera包将不同的mysql实例连接起来，实现了multi-master的集群架构。

PXC基于[Galera协议](https://blog.csdn.net/qq_38125183/article/details/80861925)，Galera是Codership提供的多主数据同步复制机制，可以实现多个节点间的数据同步复制以及读写，并且可保障数据库的服务高可用及数据一致性。

### 架构示意图：



![pxc overview](https://tva1.sinaimg.cn/large/00831rSTly1gcbvo5cearj30e70ao3zi.jpg)

图解：底层采用wsrep API接口，实现数据在多个节点之间同步复制

![](https://tva1.sinaimg.cn/large/00831rSTly1gcbvphr55lj30m80dc0ty.jpg)

图解：一次写入操作中，数据在各个节点之间的验证/回滚流程

多主复制：

多主复制意味着您可以写入任何节点，并确保该写入对于集群中的所有节点都是一致的。这与常规的MySQL复制不同，在常规的MySQL复制中，您必须对主服务器执行写操作以确保将其同步。

使用多主复制时，任何写入要么在所有节点上提交，要么根本不提交。下图显示了它对于两个节点的工作方式，但是对集群中任意数量的节点都应用了相同的逻辑。

### PXC的优点：

- 服务高可用；
- 数据同步复制(并发复制)，几乎无延迟；
- 多个可同时读写节点，可实现写扩展，不过最好事先进行分库分表，让各个节点分别写不同的表或者库，避免让galera解决数据冲突；
- 新节点可以自动部署，部署操作简单；
- 数据严格一致性，尤其适合电商类应用；
- 完全兼容MySQL；

### PXC局限性：

- 只支持InnoDB引擎；
- 所有表都要有主键；
- 不支持LOCK TABLE等显式锁操作；
- 锁冲突、死锁问题相对更多；
- 不支持XA；
- 集群吞吐量/性能取决于短板；
- 新加入节点采用SST时代价高；
- 存在写扩大问题；
- 如果并发事务量很大的话，建议采用InfiniBand网络，降低网络延迟；

事实上，采用PXC的**主要目的是解决数据的一致性问题**，高可用是顺带实现的。因为PXC存在写扩大以及木桶短板效应，并发效率会有较大损失，类似半同步复制（semi sync replication）机制。