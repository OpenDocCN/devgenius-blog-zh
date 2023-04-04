# 建立阿帕奇皮诺和普雷斯托

> 原文：<https://blog.devgenius.io/building-apache-pinot-and-presto-bf91388c7df5?source=collection_archive---------5----------------------->

## 逐步使用流式数据库

![](img/9b8dac9a6b55f5fb8ea68c0cf54c59b7.png)

丹·迈耶斯在 [Unsplash](https://unsplash.com/photos/tq2ClHwDbD8) 上拍摄的照片

最近，我们一直在调查一些流数据库解决方案，主要目标是 [Apache Pinot](https://pinot.apache.org/) ，根据描述，这符合我们的需求，因此是主要目标。

> *Apache Pinot，一种实时分布式 OLAP 数据存储，专为低延迟高吞吐量分析而构建，非常适合面向用户的分析工作负载。*

但是有些[官方文件](https://docs.pinot.apache.org/)其实是缺失的，所以我在网上找了一些资料，终于能够做一些实验了。这些过程当时没有记录，所以我不确定实际使用了什么参考资料。

目前整个实验基础设施都是用`docker-compose`搭建的，Github 库中的完整代码如下。[https://github.com/wirelessr/pinot-plus-presto](https://github.com/wirelessr/pinot-plus-presto)T21

# 阿帕奇皮诺

先解释核心部件，我们来看看`docker-compose.yml`。

```
services:
  zookeeper:
    image: zookeeper:3.5.6

  pinot-controller:
    image: apachepinot/pinot:0.9.3
    ports:
      - "9000:9000"
      - "8888"
    environment:
      JAVA_OPTS: "-javaagent:/opt/pinot/etc/jmx_prometheus_javaagent/jmx_prometheus_javaagent-0.12.0.jar=8888:/opt/pinot/etc/jmx_prometheus_javaagent/configs/pinot.yml -Dplugins.dir=/opt/pinot/plugins -Xms1G -Xmx4G -XX:+UseG1GC -XX:MaxGCPauseMillis=200 -Xloggc:gc-pinot-controller.log"

  pinot-broker:
    image: apachepinot/pinot:0.9.3
    ports:
      - "8099:8099"
      - "8888"
    environment:
      JAVA_OPTS: "-javaagent:/opt/pinot/etc/jmx_prometheus_javaagent/jmx_prometheus_javaagent-0.12.0.jar=8888:/opt/pinot/etc/jmx_prometheus_javaagent/configs/pinot.yml -Dplugins.dir=/opt/pinot/plugins -Xms4G -Xmx4G -XX:+UseG1GC -XX:MaxGCPauseMillis=200 -Xloggc:gc-pinot-broker.log"

  pinot-server:
    image: apachepinot/pinot:0.9.3
    ports:
      - "8098:8098"
      - "8888"
    environment:
      JAVA_OPTS: "-javaagent:/opt/pinot/etc/jmx_prometheus_javaagent/jmx_prometheus_javaagent-0.12.0.jar=8888:/opt/pinot/etc/jmx_prometheus_javaagent/configs/pinot.yml -Dplugins.dir=/opt/pinot/plugins -Xms4G -Xmx16G -XX:+UseG1GC -XX:MaxGCPauseMillis=200 -Xloggc:gc-pinot-server.log"
```

以上是构建 Pinot 所需的四个基本服务，大部分与官方文档相同的都被省略了，只列出了我修改过的部分。对了，我已经把图片版换成新的了，不过还是让我们在例子里保留正式版吧。

主要的变化是对三种皮诺服务的`ports`和`environment`的修改，以便普罗米修斯公司可以使用皮诺的指标。

因此，`ports`打开一个额外的`8888`，并给`JAVA_OPTS`增加一个额外的`javaagent`参数。`javaagent`的目的是使最初的皮诺 JMX 指标能够基于网络并向普罗米修斯公司公开。

到目前为止，官方文档中的`jmx_prometheus_javaagent.jar`并没有包含版本号，但是我从官方容器中找不到对应的文件，所以只能用`jmx_prometheus_javaagent-0.12.0.jar`来代替。

# 监视

为了观察服务的状态，我们还需要构建`Prometheus`和`Grafana`。

```
prometheus:
    image: prom/prometheus
    container_name: monitoring-prometheus
    restart: unless-stopped
    volumes:
    - monitoring-prometheus-data-1:/prometheus
    - ./volumes/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
    - "9090:9090"
  grafana:
    image: grafana/grafana
    volumes:
    - ./volumes/grafana/provisioning:/etc/grafana/provisioning
    - ./volumes/grafana/dashboards:/var/lib/grafana/dashboards
    ports:
    - "3000:3000"
    environment:
      GF_SERVER_ROOT_URL: https://localhost:3000
      GF_SECURITY_ADMIN_PASSWORD: password
```

在`volumes`中，我们定义了这两个服务所需的配置文件。

*   Prometheus 需要知道从哪里获取指标，因此它需要设置三个 Pinot 服务的 FQDNs。
*   Grafana 需要数据源设置和 Pinot 专用仪表板。

运行`docker-compose`后，我们可以从`https://localhost:3000`查看仪表盘。账号和密码简单来说就是`admin`和`password`。

# 阿帕奇普雷斯托

最后一个服务是 [Presto](https://prestodb.io/) 。

我们为什么需要 Presto？因为 Pinot 支持 SQL，但实际上 Pinot 不支持`JOIN`。如果需要合并两个表，必须由外部 SQL 引擎提供。

Presto 是一个 SQL 引擎，支持 ANSI SQL 和多个数据源，当然也包括 Pinot，所以我选择在我的实验中使用 Presto。

```
presto-coordinator:
    image: apachepinot/pinot-presto
    restart: unless-stopped
    ports:
    - "18080:8080"
```

Presto 有两个角色，协调者和工作者。在实验环境中，我们简单地使用协调器的内置 worker。

官方容器`apachepinot/pinot-presto`默认为协调器，没有任何设置，已经使用了 FQDN `pinot-controller:9000`，所以不需要做任何改动，我们可以直接使用。

# 结论

在克隆了 Github 存储库之后，我们可以直接运行`docker-compose up -d`来让实验环境正常运行。

整个实验环境包含三个关键点。

1.  Apache Pinot:流式数据库本身。
2.  Prometheus 和 Grafana:模拟生产环境的监控系统。
3.  Apache Presto:Pinot 中没有的查询功能。

对了，官方文档并没有介绍如何使用有安全的卡夫卡，只是以没有安全的卡夫卡为例。

在[合流](https://www.confluent.io/)的情况下，卡夫卡内置了`SASL_SSL`，我参考这篇文章来设置卡夫卡的账号和密码。
[https://dev.startree.ai/docs/pinot/recipes/kafka-sasl](https://dev.startree.ai/docs/pinot/recipes/kafka-sasl)

这个实验环境可能会根据我的一些需求进行扩展，比如添加 Presto workers。