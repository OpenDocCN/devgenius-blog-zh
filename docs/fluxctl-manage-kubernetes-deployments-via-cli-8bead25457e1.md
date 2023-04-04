# fluxctl —通过 CLI 管理 Kubernetes 部署

> 原文：<https://blog.devgenius.io/fluxctl-manage-kubernetes-deployments-via-cli-8bead25457e1?source=collection_archive---------9----------------------->

使用$ fluxctl 控制和自动化部署，这是一个方便的命令行实用程序，可以与 Kubernetes 的连续交付运营商 Flux 进行对话。

[Weave Flux](https://github.com/weaveworks/flux) 是 Kubernetes GitOps 运营商(阅读更多关于[GitOps](https://www.weave.works/technologies/gitops/)连续交付的信息)，它为您管理部署。如果你完全是编织通量的新手，你可能想看看我们的[入门教程](https://github.com/weaveworks/flux/blob/master/site/get-started.md)或我们的[头盔用户指南](https://github.com/weaveworks/flux/blob/master/site/helm-get-started.md)。

fluxctl 是一个可以与 Weave Flux 对话的命令行工具，它使管理、自动化甚至回滚部署变得非常容易。

# 安装 fluxctl 变得更加容易！

在 Mac OS 上

如果你在集群中使用 Weave Flux，并且你在 Mac 上，那么在命令行上与 Flux 交互就变得容易多了。简单地跑

```
brew install fluxctl
```

# 将`fluxctl`连接到守护进程

默认情况下，`fluxctl`会尝试将端口转发到您的 Flux 实例，假设它运行在`"default"`名称空间中。您可以用`--k8s-fwd-ns`标志指定一个不同的名称空间:

```
fluxctl --k8s-fwd-ns=weave list-workloads
```

# 工作量

# 什么是工作负载？

该术语指的是负责从版本化映像创建容器的任何集群资源——在 Kubernetes 中，这些是诸如部署、DaemonSets、StatefulSets 和 CronJobs 之类的对象。

# 查看工作负载

首先要做的是检查 Flux 是否可以看到任何正在运行的工作负载。为此，使用`list-workloads`子命令:

```
$ fluxctl list-workloads
WORKLOAD                       CONTAINER   IMAGE                                         RELEASE  POLICY
default:deployment/helloworld  helloworld  quay.io/weaveworks/helloworld:master-a000001  ready
                               sidecar     quay.io/weaveworks/sidecar:master-a000002
default:deployment/busybox     busybox     busybox:1.31.1                                ready
default:deployment/nginx       nginx       nginx:stable-alpine                           ready
```

请注意，实际运行的映像将取决于您的集群。

您还可以使用`--container|-c`选项按容器名称过滤工作负载:

```
$ fluxctl list-workloads --container helloworld
WORKLOAD                       CONTAINER   IMAGE                                         RELEASE  POLICY
default:deployment/helloworld  helloworld  quay.io/weaveworks/helloworld:master-a000001  ready
                               sidecar     quay.io/weaveworks/sidecar:master-a000002
```

# 检查容器的版本

一旦我们有了工作负载列表，我们就可以开始检查哪个版本的映像正在运行。

```
$ fluxctl list-images --workload default:deployment/helloworld
WORKLOAD                       CONTAINER   IMAGE                          CREATED
default:deployment/helloworld  helloworld  quay.io/weaveworks/helloworld
                                           |   master-9a16ff945b9e        20 Jul 16 13:19 UTC
                                           |   master-b31c617a0fe3        20 Jul 16 13:19 UTC
                                           |   master-a000002             12 Jul 16 17:17 UTC
                                           '-> master-a000001             12 Jul 16 17:16 UTC
                               sidecar     quay.io/weaveworks/sidecar
                                           '-> master-a000002             23 Aug 16 10:05 UTC
                                               master-a000001             23 Aug 16 09:53 UTC
```

箭头将指向当前运行的版本，旁边是其他版本及其时间戳的列表。

当在脚本中使用`fluxctl`时，您可以使用`--no-headers`删除表格标题，对于`list-images`和`list-workloads`命令来抑制标题:

```
$ fluxctl list-workloads --no-headers
default:deployment/helloworld  helloworld  quay.io/weaveworks/helloworld:master-a000001  ready
                               sidecar     quay.io/weaveworks/sidecar:master-a000002
$ fluxctl list-images --workload default:deployment/helloworld --no-headers
default:deployment/helloworld  helloworld  quay.io/weaveworks/helloworld
```

# 释放工作负荷

我们现在可以继续用`release`子命令更新工作负载。这将检查每个工作负载是否需要更新，如果需要，将新的配置写入存储库。

```
$ fluxctl release --workload=default:deployment/helloworld --user=phil --message="New version" --update-all-images
Submitting release ...
Commit pushed: 7dc025c
Applied 7dc025c61fdbbfc2c32f792ad61e6ff52cf0590a
WORKLOAD                     STATUS   UPDATES
default:deployment/helloworld  success  helloworld: quay.io/weaveworks/helloworld:master-a000001 -> master-9a16ff945b9e$ fluxctl list-images --workload default:deployment/helloworld
WORKLOAD                       CONTAINER   IMAGE                          CREATED
default:deployment/helloworld  helloworld  quay.io/weaveworks/helloworld
                                           '-> master-9a16ff945b9e        20 Jul 16 13:19 UTC
                                               master-b31c617a0fe3        20 Jul 16 13:19 UTC
                                               master-a000002             12 Jul 16 17:17 UTC
                                               master-a000001             12 Jul 16 17:16 UTC
                               sidecar     quay.io/weaveworks/sidecar
                                           '-> master-a000002             23 Aug 16 10:05 UTC
                                               master-a000001             23 Aug 16 09:53 UTC
```

# 打开自动化

通过`automate`子命令，可以从`fluxctl`轻松控制自动化。

```
$ fluxctl automate --workload=default:deployment/helloworld
Commit pushed: af4bf73
WORKLOAD                     STATUS   UPDATES
default:deployment/helloworld  success$ fluxctl list-workloads --namespace=default
WORKLOAD                       CONTAINER   IMAGE                                             RELEASE  POLICY
default:deployment/helloworld  helloworld  quay.io/weaveworks/helloworld:master-9a16ff945b9e ready    automated
                               sidecar     quay.io/weaveworks/sidecar:master-a000002
```

还可以通过向部署添加注释`fluxcd.io/automated: "true"`来实现自动化。

我们可以看到`list-workloads`子命令报告 helloworld 应用程序是自动化的。Flux 现在将自动部署一个可用的工作负载新版本，并将新配置提交给版本控制系统。

# 关闭自动化

使用`deautomate`命令关闭自动化:

```
$ fluxctl deautomate --workload=default:deployment/helloworld
Commit pushed: a54ef2c
WORKLOAD                     STATUS   UPDATES
default:deployment/helloworld  success$ fluxctl list-workloads --namespace=default
WORKLOAD                       CONTAINER   IMAGE                                             RELEASE  POLICY
default:deployment/helloworld  helloworld  quay.io/weaveworks/helloworld:master-9a16ff945b9e ready
                               sidecar     quay.io/weaveworks/sidecar:master-a000002
```

我们可以看到工作负载不再是自动化的。

# 回滚工作负荷

回滚可以通过以下方式实现:

*   `[deautomate](https://fluxcd.io/legacy/flux/references/fluxctl/#turning-off-automation)`防止 Flux 自动更新到新版本，以及
*   `[release](https://fluxcd.io/legacy/flux/references/fluxctl/#releasing-a-workload)`部署您想要回滚到的版本。

```
$ fluxctl list-images --workload default:deployment/helloworld
WORKLOAD                       CONTAINER   IMAGE                          CREATED
default:deployment/helloworld  helloworld  quay.io/weaveworks/helloworld
                                           '-> master-9a16ff945b9e        20 Jul 16 13:19 UTC
                                               master-b31c617a0fe3        20 Jul 16 13:19 UTC
                                               master-a000002             12 Jul 16 17:17 UTC
                                               master-a000001             12 Jul 16 17:16 UTC
                               sidecar     quay.io/weaveworks/sidecar
                                           '-> master-a000002             23 Aug 16 10:05 UTC
                                               master-a000001             23 Aug 16 09:53 UTC$ fluxctl deautomate --workload=default:deployment/helloworld
Commit pushed: c07f317
WORKLOAD                       STATUS   UPDATES
default:deployment/helloworld  success$ fluxctl release --workload=default:deployment/helloworld --update-image=quay.io/weaveworks/helloworld:master-a000001
Submitting release ...
Commit pushed: 33ce4e3
Applied 33ce4e38048f4b787c583e64505485a13c8a7836
WORKLOAD                     STATUS   UPDATES
default:deployment/helloworld  success  helloworld: quay.io/weaveworks/helloworld:master-9a16ff945b9e -> master-a000001$ fluxctl list-images --workload default:deployment/helloworld
WORKLOAD                     CONTAINER   IMAGE                          CREATED
default:deployment/helloworld  helloworld  quay.io/weaveworks/helloworld
                                           |   master-9a16ff945b9e        20 Jul 16 13:19 UTC
                                           |   master-b31c617a0fe3        20 Jul 16 13:19 UTC
                                           |   master-a000002             12 Jul 16 17:17 UTC
                                           '-> master-a000001             12 Jul 16 17:16 UTC
                               sidecar     quay.io/weaveworks/sidecar
                                           '-> master-a000002             23 Aug 16 10:05 UTC
                                               master-a000001             23 Aug 16 09:53 UTC
```

# 锁定工作负荷

锁定工作负荷将停止对该工作负荷的手动或自动释放。在文件中所做的更改仍将被同步。

```
$ fluxctl lock --workload=deployment/helloworld
Commit pushed: d726722
WORKLOAD                       STATUS   UPDATES
default:deployment/helloworld  success
```

# 将映像释放给锁定的工作负荷

可能需要将映像释放给锁定的工作负载，同时在之后保持锁定。为了不必修改锁定策略(包括作者和原因)，可以使用`--force`:

```
fluxctl release --workload=default:deployment/helloworld --update-all-images --force
```

# 解锁工作负载

解锁一个工作负载允许它拥有手动或自动的释放(再次)。

```
$ fluxctl unlock --workload=deployment/helloworld
Commit pushed: 708b63a
WORKLOAD                       STATUS   UPDATES
default:deployment/helloworld  success
```