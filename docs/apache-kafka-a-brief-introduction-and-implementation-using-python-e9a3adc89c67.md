# Apache Kafka:简介和使用 Python 的实现

> 原文：<https://blog.devgenius.io/apache-kafka-a-brief-introduction-and-implementation-using-python-e9a3adc89c67?source=collection_archive---------4----------------------->

![](img/376c6b2223d00d7da978dda7bb6dba8a.png)

阿里安·达尔维什在 [Unsplash](https://unsplash.com/) 上拍摄的照片

本文将向您简要介绍 Apache Kafka(一个消息队列)。我们首先解释什么是阿帕奇卡夫卡，为什么我们需要它超过休息。然后，我们开始解释阿帕奇卡夫卡的组成部分。之后，我们用代码(使用 **Python** )来实现 Apache Kafka。

# **先决条件**

1.  计算机编程语言
2.  机器上安装的 Docker
3.  Docker 的背景
4.  创造力永远是加分项😃

# **什么是阿帕奇卡夫卡？**

A pache Kafka 是一个开源的事件流媒体平台。它最初是由 Linkedin 开发的，然后被命名为 Apache 软件基金会。它是用 Scala 和 Java 编写的，每天可以处理数万亿个事件。

> Kafka 是一个分布式系统，由使用高性能 TCP 网络协议进行通信的**服务器和**客户端组成。它可以部署在虚拟机和容器上。 ***Kafka 服务器*** 作为一个或多个服务器的集群运行。其中一些服务器可以形成一个存储层，称为代理。其他服务器运行 Kafka connect 来持续地将事件流数据导入和导出到您的关系数据库。 ***Kafka Clients*** 允许你运行分布式应用和微服务，它们将并行地读取、写入和处理事件流。我们可以使用 Kafka 社区为 Go、Python、C/C++和许多其他编程语言以及 REST APIs 提供的库来启动我们的 Kafka 客户端。

# 卡夫卡的能力

Apache Kafka 提供了三个关键功能，因此您可以使用一个经过实战检验的解决方案来实现端到端的事件流用例:

1.  **发布和订阅**—Kafka 中的事件流使用提交日志方法来存储来自发布者的大量数据。我们可以订阅实时应用程序，使用 Kafka 主题获取实时数据。这样，Kafka 就充当了分布式系统的发布和订阅机制。
2.  **存储** — Kafka 通过将数据分布在多个节点上，以容错方式存储记录流，从而根据可用性区域在单个或多个数据中心内实现高可用性部署。
3.  **处理** —现在是处理来自事件流的数据的时候了。我们可以处理来自事件的数据。如果需要的话储存在我们的数据库里。

# 微服务的 REST 与消息传递

在解释 Kafka 组件之前，让我们探索一下请求-响应(REST)或事件流(Kafka)中的哪个模型适合我们的用例。在设计我们的系统之前，我们需要考虑几个因素。让我们根据模型分别研究这些因素，如下所示:

## 使用 REST 时的注意事项

*   **紧密耦合—** 围绕接口(特别是围绕数据)总会有一些服务耦合，但是当构建 RESTful 服务时，开发人员假设消息只需要被传递到一个地方(服务)。当另一个服务或组件将来上线并需要数据时会发生什么？当然，我们需要更新代码来添加新的端点，但是这显示了缺陷:不必要的耦合。在这些场景中，我们需要构建 RESTful APIs 来重复服务于我们的目的。
*   **阻塞** —当调用 REST 服务时，我们的服务被阻塞，等待响应。这会降低应用程序的性能，因为该线程可能正在处理其他请求。因此，当线程在等待 RESTful 服务响应的应用程序中被阻塞时，就很难管理了！
*   **无状态**—REST API 是无状态的，这意味着调用可以彼此独立地进行，并且每个调用包含成功完成自身所需的所有数据

## 面向事件驱动的微服务的消息传递(Kafka)

*   **松耦合** —使用消息传递，特别是发布/订阅模型，服务不知道其他服务。他们会收到新事件的通知，然后处理这些信息并发布新信息。这些新信息可以被任意数量的服务使用。这种松散耦合允许微服务始终可用于应用程序中永无止境的变化。
*   **非阻塞**—对于微服务来说，让许多阻塞线程等待响应是一种资源浪费。使用异步消息传递应用程序，我们可以发送一个请求并处理另一个请求，而不是等待响应。
*   **易于扩展**—微服务架构有助于我们随着应用的增长扩展应用。因为每个服务都很小，并且只执行一项任务，所以每个服务都应该能够根据需要增长或收缩。事件驱动的架构和消息模型使微服务易于扩展，因为它们是解耦的，不会阻塞。
*   **分布式事务—** 事件驱动架构帮助我们跨不同的数据库实例执行数据库事务。每个服务在执行数据库事务时可以发布一个事件，而另一个服务可以订阅该事件并执行其数据库事务。如果出现任何问题，它可以发布一个新事件，第一个服务可以订阅该事件并回滚其事务。通过使用事件驱动架构，分布式事务可以很好地实现。

# 阿帕奇卡夫卡建筑

现在，是时候简单解释一下卡夫卡了。先来解释一下下面阿帕奇卡夫卡的聚类图。

![](img/6c83d50525ef862351813b4cf1a80109.png)

卡夫卡建筑

*   **卡夫卡生产者—** 生产者充当发送者。它负责发送消息或数据。它不直接向消费者发送消息。它将消息推送到 Kafka 服务器或代理。消息或数据存储在 Kafka 服务器或代理中。多个生产者可以向同一个 Kafka 主题或不同的 Kafka 主题发送一个消息。
*   **卡夫卡消费者—** 消费者充当接收者。它负责接收或消费消息。但它不消费或直接接收来自卡夫卡生产者的信息。Kafka Producer 将消息推送到 Kafka 服务器或代理。消费者可以通过发送 Kafka 主题或订阅 Kafka 主题来请求来自 Kafka 经纪人的消息。
*   **Kafka Brokers—**Kafka Broker 只不过是一台服务器(或一台机器)。代理只是生产者和消费者之间交换消息的中间实体。对卡夫卡生产者来说，它充当的是接受者，对卡夫卡消费者来说，它充当的是发送者。在 Kafka 集群中，可以有一个或多个 Kafka 经纪人。
*   **Kafka 集群—** 分布式系统中的集群只不过是一组为共同目的而工作的服务器。同理，Kafka 集群也不过是一群代理(服务器)。Kafka 集群中可以有一个或多个代理。只有一个代理的 Kafka 集群称为**单代理集群**，有两个或更多代理的 Kafka 集群称为**多代理集群**。
*   **Kafka Topic —** Kafka Topic 只不过是赋予数据流或消息流的唯一名称。现在让我们明白这样做的必要性。正如我们所知，Kafka Producer 向代理发送消息流，Kafka Consumer 从该代理接收消息流。假设一个消费者想要使用来自 Broker 的消息，那么它将订阅 Kafka Broker 中的特定 Kafka 主题，所有到达该主题的消息都将被传递给消费者。所以基本上，Kafka 生产者生产对应于特定 Kafka 主题的数据流，Kafka 消费者消费对应于特定 Kafka 主题的数据流。
*   **Kafka Partitions —** 现在我们知道，生产者用一个称为 Kafka 主题的惟一标识符向代理发送数据，代理用该主题存储消息。现在问题来了，当 Kafka 生产者产生大量数据流时，Kafka broker 的数据存储如何管理？因为我们已经知道 Kafka 是分布式系统，所以它会将 Kafka 主题分成若干部分，并将它们分布到不同的机器上进行存储。基于用例和数据量，我们可以在 Kafka 的主题创建过程中决定主题的分区数量。
*   **偏移量—** 在 Kafka 中，Kafka 主题的每个分区中的每个消息都被分配了一个序列号。这个序列号称为偏移量。任何消息一到达分区，就给该消息分配一个编号。对于给定的主题，不同的分区有不同的偏移量。最初，偏移指针指向第一条消息。一旦消费者读取该消息，偏移指针就移动到序列中的下一条消息，依此类推。因此，Kafka 主题、分区号和偏移号的组合是任何消息的唯一标识符。
*   **消费群体—** 顾名思义，消费群体就是一群消费者。多个消费者联合起来分担工作负载。这就像将一项大任务分配给多个个体。可以有多个消费者团体订阅相同或不同的主题。属于同一消费者组的两个或更多消费者没有接收到共同的消息。他们总是收到不同的消息，因为一旦消息被该消费者组中的任何消费者消费，偏移指针就移动到下一个数字。
*   **动物园管理员—** 动物园管理员是卡夫卡的先决条件。由于 Kafka 是一个分布式系统，它使用 Zookeeper 来协调和跟踪 Kafka 集群节点的状态。它维护着一个卡夫卡主题和信息的列表。

# **实施**

![](img/bbca0c6c750be9e5330ee5bdce966e9c.png)

我们将启动我们的 Kafka 生产者，它将发送用户的信息，我们的 Kafka 消费者正在消费这些信息并进行一些操作。让我们按照下面提到的步骤进行:

*   **启动我们的 Zookeeper —** 我们将使用 docker 命令提取 Zookeeper 的图像，并使用以下命令在本地运行容器:

```
// We are running our zookeeper on port 2181 and given the container   name as zookeeper.docker run --name zookeeper -p 2181:2181 -d zookeeper**NOTE: Use port 2181 for Zookeeper**
```

![](img/0fd63f8d1dc25322fa91337c172a54b1.png)

我们可以使用 docker logs 命令来检查我们的 zookeeper 容器是否正在运行，也就是说，docker logs<container_id></container_id>

*   **运行 Kafka 集群—** 我们将提取 Kafka docker 映像，并使用以下命令运行容器:

```
// We are running single kafka broker.
// In this command, we need to specify the port on which we have run our zookeeper i.e, in our case its 2181.docker run -p 9092:9092 --name kafka  
        -e KAFKA_ZOOKEEPER_CONNECT=**shubham-mac**:2181 
        -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://**shubham-mac**:9092 
        -e KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=1 
        -d confluentinc/cp-kafka**NOTE: Use port 9092 for kafka server and add your docker host name from file /etc/hosts**
```

*   **创建我们的 Kafka 主题—** 现在，为了创建 Kafka 主题，我们需要创建一个 python 脚本并指定主题名称。

```
*# importing required packages* from kafka.admin import KafkaAdminClient, NewTopic

*# Add kafka-server host and port (docker hostname)* server = **"shubham-mac:9092"** kafka_admin = KafkaAdminClient(bootstrap_servers=server)

*# Creating a new Kafka-topic = Users* topic_list = list()
topic_list.append(NewTopic(name=**"Users"**, num_partitions=1, replication_factor=1))
kafka_admin.create_topics(new_topics=topic_list, validate_only=False)
```

*   **创建我们的 Kafka Producer —** 现在，我们将使用 Kafka Producer 向特定的 Kafka 主题发送用户详细信息。

```
*# importing kafka producer* from kafka.producer import KafkaProducer
import json

*# Add kafka-server host and port (docker hostname)* server = **"shubham-mac:9092"** kafka_producer = KafkaProducer(bootstrap_servers=server)

*# Kafka Topic that we have created in previous step* topic = **"Users"** *# Data to be send to Kafka* data = {"name": "Shubham Kaushik", "profession": "Software Developer"}

*# Sending Data* kafka_producer.send(topic, json.dumps(data, indent=2).encode(**'**utf-8'))

kafka_producer.flush()
```

*   **创建我们的 Kafka 消费者—** 现在，我们将使用 Kafka 消费者来消费来自“用户”Kafka 主题的数据。

```
*# importing kafka producer* from kafka.consumer import KafkaConsumer
import json

*# Add kafka-server host and port (docker hostname)* server = **"shubham-mac:9092"** *# Kafka Topic that our Kafka Consumer is subscribing.* topic = **"Users"** consumer = KafkaConsumer(topic, bootstrap_servers=server)

*# Consuming Data and when we received data we are printing it to console* try:
    for msg in consumer:
        if msg.value:
            print(**"**Data Received**"**, json.loads(msg.value))
finally:
    *# Close down consumer to commit final offsets.* consumer.close()
```

# 恭喜你。🙂

*我们已经成功运行了我们的 Kafka Producer，Kafka Consumer 也创建了我们的 Kafka 主题。*

![](img/be1ea93e8ffe955dcdcc187244938247.png)

当我们使用 Kafka 来完成我们的生产就绪应用程序的用例时，许多其他因素也发挥了作用。其中一些因素如下:

1.  容错
2.  卡夫卡主题分发和复制
3.  监督我们的卡夫卡生产者和消费者
4.  阅读事务性消息

> *如果您错过了任何步骤，请遵循*[*GitHub*](https://github.com/ishubham169/apache_kafka_python_implementation)*上的代码。*

# 摘要

为了这篇文章的完整性，让我们快速回顾一下到目前为止我们所学的内容。

1.  阿帕奇卡夫卡是什么，用来做什么？
2.  卡夫卡的能力。
3.  我们知道了为什么休息并不总是最好的选择。
4.  卡夫卡建筑。
5.  用 Python 实现了 Kafka。

> 如果你喜欢这篇文章，别忘了给它一个掌声！

![](img/a7d8c1ca5201216f8856b10e902b2fba.png)

请随时在[**Linkedin**](https://www.linkedin.com/in/shubham-kaushik-temp/)**上 ping 我，敬请期待下一期！**

# **参考**

**[1]阿帕奇卡夫卡官网[https://www.confluent.io/](https://www.confluent.io/what-is-apache-kafka/)**

**[2]卡夫卡文献[https://kafka.apache.org/documentation/#gettingStarted](https://kafka.apache.org/documentation/#gettingStarted)**

**[3]码头工人中心[https://hub.docker.com/r/bitnami/kafka/](https://hub.docker.com/r/bitnami/kafka/)**

**[4] AWS 卡夫卡文件[https://aws.amazon.com/msk/what-is-kafka/](https://aws.amazon.com/msk/what-is-kafka/)**