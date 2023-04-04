# 普罗米修斯舱坠毁——被杀死

> 原文：<https://blog.devgenius.io/prometheus-pod-crashes-oomkilled-8ccb68c20909?source=collection_archive---------4----------------------->

# 症状

普罗米修斯号仍处于坠毁状态。当您发出以下命令来获取 pod 细节时，Prometheus 容器终止，原因是，`OOMKilled`

> 库贝克特尔描述吊舱普罗米修斯-k8s-0 -n 监控

# 原因

问题是，普罗米修斯正在经历高工作量。大量的度量数据需要比 Prometheus 容器上当前可用的内存更多的内存。

# 解决问题

以下选项可以解决这个问题。

*   可以减少现有的普罗米修斯工作量。通过减小刮擦间隔值来降低[刮擦](https://prometheus.io/docs/prometheus/latest/getting_started/#configuring-prometheus-to-monitor-itself)频率
*   你可以增加普罗米修斯容器上的内存限制。
*   您的 Prometheus 容器继续崩溃，并且日志中没有出现错误消息。这种情况可能表明您的 Prometheus 容器内的`/prometheus/wal path`中剩余了太多数据。要解决此问题，请从容器中删除`/wal`文件夹和数据段。

> k exec-it Prometheus-k8s-0-c Prometheus-RM-RF wal/
> 
> k exec-it Prometheus-k8s-0-c Prometheus-RM-RF *

**注**:删除`/wal folder can lead to data loss.`