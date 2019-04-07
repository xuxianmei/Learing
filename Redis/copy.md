# 复制
在分布式系统中为了解决单点问题，通常会把数据复制多个副本部署
到其他机器，满足故障恢复和负载均衡等需求。

Redis也是如此，它为我们提供了复制功能，实现了相同数据的多个
Redis副本。复制功能是高可用Redis的基础。

## 配置

参与复制的Redis实例划分为主节点（master）和从节点（slave）。
默认情况下，Redis都是主节点。
对应关系：master（1---n）slave，其中n>=0。

复制的数据流是单向的，只能由主节点复制到从节点。


### 建立复制

配置复制的方式有以下三种：

1. 在配置文件中加入slaveof{masterHost}{masterPort}随
Redis启动生效。

2. 在redis-server启动命令后加入--slaveof{masterHost}
{masterPort}生效。

3. 直接使用命令：slaveof{masterHost}{masterPort}生效。

slaveof本身是异步命令，执行slaveof命令时，节点只保存主节点
信息后返回，后续复制流程在节点内部异步执行。
主从节点复制成功建立后，可以使用info replication命令查看复制相关
状态。

### 断开复制

从节点执行slaveof no one断开与主节点复制关系。


断开复制主要流程：
1. 断开与主节点复制关系。
2. 从节点晋升为主节点。

切主操作：
通过slaveof命令还可以实现切主操作。
所谓切主是指把当前从节点对主节点的复制切换到另一个主节点。
执行slaveof{newMasterIp}{newMasterPort}命令即可。

切主操作流程如下：
1. 断开与旧主节点复制关系。
2. 与新主节点建立复制关系。
3. 删除从节点当前所有数据。
4. 对新主节点进行复制操作。

注：从节点使用slave-read-only=yes配置为只读模式。


## 拓扑结构（TO DO）

### 一主一从

### 一主多从

### 树状主从

## 原理

在从节点执行slaveof命令后，复制过程便开始运作。
下面详细介绍建立复制的完整流程：

1. 保存主节点（master）信息
执行slaveof后从节点只保存主节点的地址信息便直接返回，
这时建立复制流程还没有开始。

2. 从节点（slave）内部通过每秒运行的定时任务维护复制相关逻
辑，当定时任务发现存在新的主节点后，会尝试与该节点建立网络连接。

3. 发送ping命令
连接建立成功后从节点发送ping请求进行首次通信，ping请求主要目的如下：
	* 检测主从之间网络套接字是否可用。

	* 检测主节点当前是否可接受处理命令
4. 权限验证

5. 同步数据集（全量同步和部分同步）

6. 命令持续复制。


### 数据同步

* 全量复制
一般用于初次复制场景，Redis早期支持的复制功能只有全量复制，
它会把主节点全部数据一次性发送给从节点，当数据量较大时，
会对主从节点和网络造成很大的开销。

* 部分复制
用于处理在主从复制中因网络闪断等原因造成的数据丢失场景，
当从节点再次连上主节点后，如果条件允许，主节点会补发丢失数据
给从节点。因为补发的数据远远小于全量数据，可以有效避免全量复
制的过高开销。


### 心跳检测

主从节点在建立复制后，它们之间维护着长连接并彼此发送心跳命令。


### 异步复制

主节点不但负责数据读写，还负责把写命令同步给从节点。
写命令的发送过程是异步完成，也就是说主节点自身处理完写命令后
直接返回给客户端，并不等待从节点复制完成。

### 开发与运维中的问题（TO DO）