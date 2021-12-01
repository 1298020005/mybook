## Kafka**基础**

Kafka 是一套流处理系统，可以让后端服务轻松的相互沟通，是微服务架构中常用的组件。

![img](https://pic2.zhimg.com/80/v2-202fdc491711ad66b1453ae0a41dae05_1440w.jpg)

## **生产者消费者**

生产者服务 Producer 向 Kafka 发送消息，消费者服务 Consumer 监听 Kafka 接收消息。

![img](https://pic3.zhimg.com/80/v2-bd1cd31340c396debb1ef546bf65dfde_1440w.jpg)

一个服务可以同时为生产者和消费者。

![img](https://pic3.zhimg.com/80/v2-409e1c67c61e902948ebe86a564bb8b6_1440w.jpg)

## **Topics 主题**

Topic 是生产者发送消息的目标地址，是消费者的监听目标。

![img](https://pic3.zhimg.com/80/v2-1eceb4009428e951aa9a93e2e5217bda_720w.jpg)

一个服务可以监听、发送多个 Topics。

![img](https://pic4.zhimg.com/80/v2-82d99ef171de7f0c4e467dc8618af9a7_720w.jpg)

Kafka 中有一个【consumer-group（消费者组）】的概念。

这是一组服务，扮演一个消费者。

![img](https://pic4.zhimg.com/80/v2-c665a5156438d5d9d9bea7c429bb6087_720w.jpg)

如果是消费者组接收消息，Kafka 会把一条消息路由到组中的某一个服务。

![img](https://pic2.zhimg.com/80/v2-4e5348860bb811c94fcdfa165c5f9ddd_720w.jpg)

这样有助于消息的负载均衡，也方便扩展消费者。

Topic 扮演一个消息的队列。

首先，一条消息发送了。

![img](https://pic2.zhimg.com/80/v2-1a448ab0bca8bb2c504ca7561cea5a0d_720w.jpg)

然后，这条消息被记录和存储在这个队列中，不允许被修改。

![img](https://pic3.zhimg.com/80/v2-f70a37672ed4bb75ac351b13c49edd0a_720w.jpg)

接下来，消息会被发送给此 Topic 的消费者。

但是，这条消息并不会被删除，会继续保留在队列中。

![img](https://pic2.zhimg.com/80/v2-9618d3cea1cb8b591e9ee22fcbc377b5_720w.jpg)

继续发送消息。

![img](https://pic3.zhimg.com/80/v2-a66b02a1f0881ae65bc9b2a0cb45eae2_720w.jpg)

像之前一样，这条消息会发送给消费者、不允许被改动、一直呆在队列中。

（消息在队列中能呆多久，可以修改 Kafka 的配置）

![img](https://pic1.zhimg.com/80/v2-a0d3b5530f5af37622f53b1a1c2a1e24_720w.jpg)

![img](https://pic4.zhimg.com/80/v2-10d8593518037bbc0735b85d899dc467_720w.jpg)

## **Partitions 分区**

上面 Topic 的描述中，把 Topic 看做了一个队列，实际上，一个 Topic 是由多个队列组成的，被称为【Partition（分区）】。

这样可以便于 Topic 的扩展。

![img](https://pic2.zhimg.com/80/v2-3017135e22a7c37f980082082c032f3d_720w.jpg)

生产者发送消息的时候，这条消息会被路由到此 Topic 中的某一个 Partition。

![img](https://pic1.zhimg.com/80/v2-9da163d4196f3944e86a608f824eac90_720w.jpg)

消费者监听的是所有分区。

![img](https://pic3.zhimg.com/80/v2-9bd2d18d98e52a4e01ced9c771fb5b56_720w.jpg)

生产者发送消息时，默认是面向 Topic 的，由 Topic 决定放在哪个 Partition，默认使用轮询策略。

![img](https://pic4.zhimg.com/80/v2-9e158769ac3de0769a5ef066e188d05f_720w.jpg)

也可以配置 Topic，让同类型的消息都在同一个 Partition。

例如，处理用户消息，可以让某一个用户所有消息都在一个 Partition。

例如，用户1发送了3条消息：A、B、C，默认情况下，这3条消息是在不同的 Partition 中（如 P1、P2、P3）。

在配置之后，可以确保用户1的所有消息都发到同一个分区中（如 P1）。

![img](https://pic2.zhimg.com/80/v2-299deeab52c84bdf4a984baf4629d975_720w.jpg)

这个功能有什么用呢？

这是为了提供消息的【有序性】。

消息在不同的 Partition 是不能保证有序的，只有一个 Partition 内的消息是有序的。

![img](https://pic1.zhimg.com/80/v2-1a5f7573268951c2587e73b52e2c3a50_720w.jpg)

![img](https://pic3.zhimg.com/80/v2-82a9a5700b17791e0fd58a16b575e22a_720w.jpg)

## **架构**

Kafka 是集群架构的，ZooKeeper是重要组件。

![img](https://pic4.zhimg.com/80/v2-324b23f85df2e4f2829b6f76ffdc1dcf_720w.jpg)

ZooKeeper 管理者所有的 Topic 和 Partition。

Topic 和 Partition 存储在 Node 物理节点中，ZooKeeper负责维护这些 Node。

![img](https://pic3.zhimg.com/80/v2-5bfe8a4a438c2a6f98177f46b849d19a_720w.jpg)

例如，有2个 Topic，各自有2个 Partition。

![img](https://pic4.zhimg.com/80/v2-71802c8d55a28c271ff9f1232d7452cf_720w.jpg)

这是逻辑上的形式，但在 Kafka 集群中的实际存储可能是这样的：

![img](https://pic3.zhimg.com/80/v2-7da8e56db17af096fc757a3ad7223b9a_720w.jpg)

Topic A 的 Partition #1 有3份，分布在各个 Node 上。

这样可以增加 Kafka 的可靠性和系统弹性。

3个 Partition #1 中，ZooKeeper 会指定一个 Leader，负责接收生产者发来的消息。

![img](https://pic2.zhimg.com/80/v2-b6a244a77b1f8d0b74d37812b16f1e41_720w.jpg)

其他2个 Partition #1 会作为 Follower，Leader 接收到的消息会复制给 Follower。

![img](https://pic1.zhimg.com/80/v2-129c9e452970b8dd59cf64a4fbd71584_720w.jpg)

这样，每个 Partition 都含有了全量消息数据。

![img](https://pic3.zhimg.com/80/v2-36c5648decb5f48f7f47f3254c100242_720w.jpg)

即使某个 Node 节点出现了故障，也不用担心消息的损坏。

Topic A 和 Topic B 的所有 Partition 分布可能就是这样的：

![img](https://pic3.zhimg.com/80/v2-19c1593d575992ea89e4bfab3479cc16_720w.jpg)

