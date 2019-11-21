盘点Zookeeper在分布式架构中的应用

近日了解到 Kafka 正在酝酿重大更新，可能会提供自管理的元数据仲裁机制以消除对 Zookeeper 的依赖，社区呼吁也相当强烈。那么一般而言 Zookeeper 在分布式系统中扮演什么角色？目前 Zookeeper 都应用在哪些分布式架构中？本文从 Zookeeper 可以聊起，盘点那些离不开 Zookeeper 的分布式技术架构！

  

一. Zookeeper 概述

  

Zookeeper 是一个高性能、高可靠的分布式协调系统，是 Google Chubby 的一个开源实现。Zookeeper 能够为分布式应用提供一致性服务，提供的功能包括：配置维护、域名服务、分布式同步、组服务等。  

Zookeeper 使用 Zab 协议传递 leader 的状态改变，保证 leader 与 follower 的一致性。Zab 全称 Zookeeper Atomic Broadcast protocol，是 Paxos 共识算法的经典实现。

Zookeeper?应用非常广泛，应用场景主要包括：

*   数据发布订阅（配置中心）
    
*   命名服务（保存全局唯一ID）
    
*   分布式协调服务（Watcher、异步通知）
    
*   心跳检测（临时节点）
    
*   任务进度上报（临时节点）
    
*   Master选举（临时节点、Watcher）
    
*   分布式锁（临时节点、Watcher）
    

前面介绍说 Zookeeper 是一个高性能、高可靠的系统，之所以是高性能主要因为 Zookeeper 保存在内存中，此外 Zookeeper 通常是集群模式，不存在单点故障即保证了其可靠性。

二. Kafka 与 Zookeeper

  

Zookeeper 在 Kafka 架构中扮演着重要角色。Kafka 使用 Zookeeper 进行元数据管理，保存 broker 注册的信息，包括 Topic、Partition 信息等，选举 Partition Leader，低版本 Kafka Consumer 的 offset 信息也保存在 Zookeeper 中。  

我们从 Kafka 架构中来看 Kafka 与 Zookeeper 的关系 ，如下

来源：云栖社区

如图中所示，Kafka Producer 端直接连接 broker.list 列表。而Broker 端则使用 Zookeeper 注册 broker 信息，监控 Partition leader 存活性；Comsumer 端则使用 Zookeeper 注册 comsumer 信息，同时用来发现 broker 列表，并和 Partition leader 建立 socket 连接获取消息。

  

三. Hadoop?与?Zookeeper

  

Zookeeper 是作为 Hadoop 子项目发展起来的，最初的设计目标就是为 Hadoop 生态组件之间提供一致性协调服务的，因此在 Hadoop 体系（这里主要指 HDFS 与 YARN）中使用广泛。  

在 Hadoop 中，Zookeeper 主要用来实现 HA（High Availability），包括 HDFS NameNode 和 YARN ResourceManager 的 HA。这里就 YARN ResourceManager 的 HA 架构举例，如下

来源：Hadoop官网

如上图所示，更多的实现细节这里不多赘述了，HDFS 利用 Zookeeper 实现 NameNode ?HA 的原理同上。

四. HBase 与 Zookeeper

  

HBase 是大数据领域使用最广泛的 NoSQL 数据库，Zookeeper 在 HBase 架构中同样扮演着重要角色。我们首先来看一张 HBase 的架构图：  

来源：MAPR官网

如上所示，HBase 主要包括 HMaster、RegionServer、Zookeeper 三种角色。我们可以很清楚的看到，HBase 使用 Zookeeper 作为分布式协调服务，来维护集群中服务的状态，所有的 RegionServer 和 Active HMaster 都要与 Zookeeper 保持会话（session）。Zookeeper 维护着哪些 servers 是健康可用的，并且在 server 故障时做出通知。

五. Solr 与 Zookeeper

  

Zookeeper 在 Solr 的架构中也有非常重要的应用。生产环境中我们通常使用 SolrCloud 模式，Solr 集群依赖 Zookeeper 管理相关配置信息和集群的运行状态，并使用 Zookeeper 进行主节点选举。

提及 Solr 就不得不说说 Elasticsearch，Elasticsearch 同 Solr 一样也是非常流行的搜索引擎，但是 Elasticsearch 却不依赖 Zookeeper 做分布式协调，而是自带集群分布式协调系统（Zen Discovery 或 Zen2），这也是 Elasticsearch 与 Solr 之间的重要区别之一。

六. 写在最后

  

我们知道 Zookeeper 在分布式系统架构中使用广泛，本文重点介绍了 Zookeeper 在大数据领域分布式架构中的几个典型应用，除此之外，Zookeeper 还有更多的使用场景，包括在 Spring Cloud 微服务架构、Dubbo 分布式架构中都有应用，这里就不多赘言了。

  

参考：

*   Zookeeper原理及其在Hadoop和HBase中的应用：https://www.cnblogs.com/iyulang/p/6604914.html
    
*   kafka与zookeeper之间的关联：https://yq.aliyun.com/articles/653930
    
*   zookeeper在大型分布式系统中的应用：https://www.cnblogs.com/aoshicangqiong/p/7978418.html
    