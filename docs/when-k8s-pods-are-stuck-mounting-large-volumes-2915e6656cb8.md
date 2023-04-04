# 当 K8s pods 无法装载大量数据时

> 原文：<https://blog.devgenius.io/when-k8s-pods-are-stuck-mounting-large-volumes-2915e6656cb8?source=collection_archive---------2----------------------->

![](img/7a149feb8c25098c3d8dd84f1ffd4f03.png)

最近，我们在 AWS/EKS 上部署 Loki 时遇到了以下问题。在每次部署或重启 Loki Pod 时，安装持久卷的时间越来越长。在我们的生产集群上，开始时有几分钟的延迟，结束时将近 25 分钟。由于对此没有解决方案，我们尽可能避免新的部署，因为我们知道这不是一个可接受的变通办法。

```
Events:
Type    Reason      Age      From              Message
— — — — — — — — — — — — -
Normal  Scheduled   23m50s   default-scheduler Successfully assigned default/filecr34t0r-0 to ip-100–64–8–204.eu-central-1.compute.internal
Normal  SuccessfulAttachVolume 23m48s attachdetach-controller AttachVolume.Attach succeeded for volume “pvc-ef3366b8-464c-11ed-b878-0242ac120002”
**Warning FailedMount** **5m43s (x6 over 18m)** **kubelet Unable to attach or mount volumes: unmounted volumes=[vol], unattached volumes=[vol kube-api-access-7wzcs]: timed out waiting for the condition**
Normal  Pulled       106s   kubelet            Container image “grafana/loki:2.6.1” already present on machine
Normal  Created      106s   kubelet            Created container loki
Normal  Started      106s   kubelet            Started container loki
```

然后我开始调查这件事。在测试和生产中，我们使用自动调配的 gp3 卷。AWS 卷监视器显示在装载期间有大量的 I/O 活动。测试卷大约有 130 万个文件，装载大约需要 7 分钟。在生产时，该卷有 430 万个，装载需要 24 分钟。好的，它似乎与文件的数量相关。使用 gp3 s 3000 iOPs，我们可以进行以下计算:

*   测试:1300000/3000/60 = 7.2 分钟
*   产品 4300000/3000/60 = 23.8 分钟

通过搜索 K8s 文档和博客，我找到了解决方案:Kubernetes 递归地改变每个卷的内容的所有权和权限，以匹配挂载该卷时 Pod 的`securityContext`中指定的`fsGroup`。对于大型卷，检查和更改所有权和权限会花费大量时间，从而减慢 Pod 启动速度。

通过`securityContext`中的`fsGroupChangePolicy`字段，您可以控制 Kubernetes 检查和管理一个卷的所有权和权限的方式。可能的值:

*   *OnRootMismatch* :仅当根目录的权限和所有权与卷的预期权限不匹配时，才更改权限和所有权。这有助于缩短更改卷的所有权和权限所需的时间。
*   *Always* :挂载卷时，始终更改卷的权限和所有权。

```
template:
  spec:
    containers:
      ...
    securityContext:
      fsGroup: 10001
      runAsGroup: 10001
      runAsNonRoot: true
      runAsUser: 10001
      **fsGroupChangePolicy: "OnRootMismatch"**
```

通过这一修改，我们的 Loki 实例的启动时间变回到两分钟以内。

这只有在您的部署或 StatefulSet 已经配置了一个`securityContext`的情况下才适用，希望您已经完成了。😉

附录:大量的文件是由 Loki 产生的，这是因为自定义标签的使用过于自由。同时，我们减少了标签的数量，随着文件数量的减少，Grafana 中的查询也变得更快了。