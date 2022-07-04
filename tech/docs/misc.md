# 选主机制
1、Zab（zookeeper使用）
2、Raft协议

两种选择算法的区别联系
（1）他们都是Paxos算法的变种。
（2）与zab协议不同的是，zab的每一个节点都有被选为leader的协议，raft协议必须从候选人中选择为leader。

该方式有如下缺点： a、脑裂（split-brain）
这是由于zookeeper的特性引起的，虽然zookeeper能保证所有的watch按顺序触发，但并不能保证同一时刻所有的replica看到的状态是一样的，这就可能造成不同的replica的响应不一致；（如由于网络原因部分选择的leader已经更新，但是其他follower由于网络原因没有看到，部分又选举出一个leader，导致一个集群出现两个leader）
b、羊（惊）群效应（herd effect）
如果宕机的哪个broker上的partition比较多，会造成多个watch被触发，造成集群类大量的调整（向羊群开一枪所有羊群四处奔散，造成网路资源和负载）。 c、zookeeper负载过重：每一个replica都要为此在zookeeper上注册一个watch，当集群规模增加到几千个partition时，zookeeper负载会过重。


# zookeeper

1、数据发布/订阅（推拉结合）

2、负载均衡

3、命名服务<br/>
3-1、提供类 JNDI 功能，可以把系统中各种服务的名称、地址以及目录信息存放在 ZooKeeper，需要的时候去 ZooKeeper 中读取<br/>
3-2、制作分布式的序列号生成器（顺序节点）

4、分布式协调/通知（临时节点）<br/>
减少系统的耦合

5、集群管理

6、Master 选举

7、分布式锁

8、分布式队列<br/>
8-1、FIFO（有序节点）
