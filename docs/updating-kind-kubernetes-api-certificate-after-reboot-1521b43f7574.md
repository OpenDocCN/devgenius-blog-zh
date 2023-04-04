# 重新启动后更新种类 Kubernetes API 证书

> 原文：<https://blog.devgenius.io/updating-kind-kubernetes-api-certificate-after-reboot-1521b43f7574?source=collection_archive---------0----------------------->

## kind | kubernetes |证书| docker |重新启动

## 运行多节点`kind` K8s 集群 24/7 以了解 Kubernetes 的一切及更多信息的挑战。

![](img/f1c5a44664e8497facb89ab570f3c5f7.png)

安德里亚·贝卢奇在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 放弃

> [kind](https://sigs.k8s.io/kind) 是一个使用 Docker 容器“节点”运行本地 Kubernetes 集群的工具。
> kind 主要设计用于测试 Kubernetes 本身，但也可用于本地开发或 CI。

所以使用它要自担风险。`kind`不能全天候运行，更不能在“生产”环境中运行。我用它来方便快捷地访问 Kubernetes 集群以了解更多信息。**这个故事不包括** `**kind**` **的安装或设置，因为这很容易，并且在它的** [**网页**](https://kind.sigs.k8s.io/docs/user/quick-start/) **上有详细记录。**

# 形势

我想通过使用它来更多地了解 Kubernetes。我在网上有一台服务器，用于我的私人项目。

所以我开始在 docker 中安装 Kubernetes(称为`kind`)，因为这将允许我在一台主机上创建多个集群，并学习操作集群、通过管道部署服务、编写掌舵图等。一开始这是最简单的解决方案……但事实证明并非没有障碍。有许多替代方案(microk8s、k3s、minikube……)并且我可能有一天会尝试它们，但是我发现`kind`非常有趣的是它可以处理多个节点(docker 容器)并且易于设置。

为了真正了解 Kubernetes 是如何工作和执行的，我想要一个永久运行的集群，而这不是`kind`的设计初衷。我让它为我工作，并想在这里分享我的经验，所以它可能会帮助别人。

# 我遇到的问题

我偶然发现了多个问题，其中大部分与我有限的知识有关，但也有一些与`kind`的局限性有关

*   `kind`不允许您在集群中添加或删除节点。
*   `kind`在中断群集的重新引导/重启之后，节点可能会获得新的 IP 地址(特别是如果主节点获得了新的 IP 地址)。
*   如果在配置中使用`extraPortMappings`,它们是“固定的”,不能修改，除非您重新创建集群。
*   一些掌舵图需要使用标准端口，因此将外部端口映射到您的`kind`节点以访问部署的服务可能有效，也可能无效。例如，GitLab Helm 图表需要域上的端口`80`和`443`，因此这些端口必须可用，否则您必须使用单独的 IP 地址。
*   对动态持久卷的支持是有限的，但是您可以将本地目录附加到`kind`节点，然后通过 Kubernetes 中的`PersistentVolume`使其可用。
*   创建多个集群时，您的主机可能会遇到`inotify`限制，集群创建、pods 启动或查看日志将会失败。
*   每当您创建一个集群时，Kubernetes 配置都会被写入`.kube/config`。因此，每次重新创建集群时，您都需要更新“客户机”上的配置，也就是您运行`helm`的地方。(例如，如果您使用不同于创建集群的用户来执行此操作)。

# 我做的决定

为了在我的场景中使用`kind`,并为即将到来的项目保持灵活性(使用新端口添加新服务)，我做出了以下决定

*   仅将`extraPortMappings`用于`kind`集群，因为我知道端口将保持稳定，或者重新创建集群是更好的选择
*   相反，使用`DNAT` iptables 规则将外部端口映射到`kind`节点，这样可以更加灵活，避免为每个新端口重新创建集群。这不是最初的打算，但给了我机会去了解关于`iptables`的一切。这个解决方案确实有一些挑战(`DNAT`也必须发生在 k8s 节点上！)，我可能会在另一篇文章中写更多。

使用 iptables 规则的缺点/后果:当节点的 IP 地址改变时，它们必须更新。

# 克服`kind`限制

## 更新 API 服务器 IP 地址

当您的主机重新启动或者您的`control-plane`节点由于某种原因获得新的 IP 地址时，k8s 集群将不再正常工作。原因很简单，因为`kind`是为了测试而不是永久使用而建造的。它只创建了一次 docker 节点，没有合适的系统来更新它们。

## 解决方案:一个小的更新脚本

下面的小脚本将获取`kubeadm-config`的`advertiseAddress`，并将其与 API 节点的实际 IP 地址进行比较。如果它们不匹配，它将更新主节点的配置并重新启动它。

update_ip.sh 更新 kind 主节点上的配置

## API 证书呢？

如果您只更新配置以匹配新的 IP 地址，您将会注意到对集群 API 的请求将会失败，因为该 IP 没有在证书中列出。

```
E1229 11:20:11.721301       1 leaderelection.go:321] error retrieving resource lock kube-system/kube-controller-manager: Get "[https://172.18.0.3:6443/api/v1/namespaces/kube-system/endpoints/kube-controller-manager?timeout=10s](https://172.18.0.3:6443/api/v1/namespaces/kube-system/endpoints/kube-controller-manager?timeout=10s)": x509: certificate is valid for 10.96.0.1, 172.18.0.2, not 172.18.0.3
```

还有其他原因可能需要更新 API 证书，例如，如果您希望在不同的主机名上访问它。

同样，这里有一个小脚本，可以让你快速更新你的 API 证书，它是由上面的`update_ip.sh`脚本调用的。

update_cert.sh 添加/更新 API 证书的 IP 地址或主机名

## 额外收获:额外的 IP 地址和主机名

上面的`update_cert.sh`脚本的好处是，您可以根据需要传递任意多对`old`和`new`名称。它将在创建证书并将其上传到`control-plane`节点之前更新或添加 IP 地址或 DNS 名称。

## 备份您的让我们加密证书

如果您像我一样，在集群上使用“真正的”域和证书，那么在重新部署您的服务之前，备份您发布的证书并在 Kubernetes 集群上恢复它们会更好。

这里有一个小脚本可以帮你做到这一点:

为什么？因为我因为过于频繁地重新创建我的`kind`集群而几次遇到了“让我们加密发布限制”。每次再次颁发相同的证书，加密的限制是每个注册域 50 个证书，参见[https://letsencrypt.org/docs/rate-limits/](https://letsencrypt.org/docs/rate-limits/)。你可能会想:老兄，这家伙一天要再造他的集群 10 次！但事实并非如此。我遇到这个限制的原因是因为我使用一个包含许多子域名的域名来提供我的所有服务——但是它们都计入同一个注册的主域名。

## 使用多个 IP 地址

如果您想要运行使用相同标准端口的多个集群或独立服务，您将需要多个 IP 地址。在开发/本地/测试环境中，这绝对不是问题，因为您可以从私有地址空间使用 IPs:[https://www . Arin . net/reference/research/statistics/address _ filters/](https://www.arin.net/reference/research/statistics/address_filters/)

*   向`eth0`添加额外的 IP 地址

```
ip add 192.168.1.99 dev eth0
```

*   将服务域名解析为新 IP。显然，如果您有自己的 DNS 服务器，您会希望在那里配置它。

```
echo "192.168.1.99  my.new.service.domain" >> /etc/hosts
```

现在，您可以使用相同的标准端口，通过`extraPortMappings`或`DNAT` iptables 规则，在这个新的 IP 地址上从外部获得另一个集群/服务。

如果您有其他服务直接运行在已经占用标准端口的主机上，这也可能是一个好的解决方案。(例如网络服务器)。为您的服务器在互联网上获取额外的 IP 地址也是可能的，但通常需要一定的费用。

## 提高营养限度

如果创建额外的集群失败，或者在尝试查看容器的日志时出现错误(`failed to create fsnotify watcher: too many open files`，可能是您已经达到了主机上的`inotify`限制。增加它们并重试。
(另见[](https://github.com/kubernetes-sigs/kind/issues/904)**)**

```
*sysctl fs.inotify.max_user_instances=512
sysctl fs.inotify.max_user_watches=16384*
```

## *将本地目录用作持久卷*

*为了在重启或重新安装您的`kind`集群时持久化数据，您可以将本地目录挂载到节点中。*

```
*- role: worker
  [...]
  extraMounts:
  - hostPath: /home/kind/data
    containerPath: /data*
```

*然后通过一个或多个 PersistentVolumes 使目录(或子目录)可用。如果您有多个工作节点，请确保始终选择同一个节点。我没有尝试过，也不建议在多种类型的节点上挂载同一个目录(如果这可行的话)。*

*给节点一个额外的`storage`标签:*

```
*- role: worker
  [...]
  kubeadmConfigPatches:
  - |
    kind: JoinConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "storage=data"*
```

*并根据标签选择持久卷的节点。当 pods 使用 PersistentVolume 时，请确保只通过`nodeSelector`将 pods 部署到该节点。*

```
*---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: local-data
spec:
  accessModes:
    - ReadWriteOnce
  capacity:
    storage: 1Gi
  local:
    path: /data
  storageClassName: local-storage
  *# only on 'storage=data' nodes* nodeAffinity:
    required:
      nodeSelectorTerms:
        - matchExpressions:
          - key: storage
            operator: In
            values:
              - data*
```

*您**可以**创建默认的 Kubernetes 存储类`local-storage`，但是`local`存储提供者不支持“[动态卷供应](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/)”，因此您的最佳选择是手动创建持久卷和声明。
(参见[https://kubernetes . io/docs/concepts/storage/storage-classes/# local](https://kubernetes.io/docs/concepts/storage/storage-classes/#local))*

# *想了解更多？*

*感谢您的阅读！我感谢任何反馈。如果你想了解更多关于我的`kind`设置，请在评论中告诉我。*

*我使用这个设置来运行多个`kind` Kubernetes 集群(dev、stage、prod)并部署 cert-manager、InfluxDB、Grafana、Nginx、Postfix 等服务。通过 GitLab CI 管道中运行的舵图。一些服务将它们的数据保存在主机上，以便在重启后仍然存在。我甚至成功地将 GitLab 安装在它自己的`kind`集群中。**这是一个非生产环境，**虽然从技术上来说对我来说是“生产”环境。我没有责任，或者换句话说:没有人关心我的服务是否中断了几个小时…除了我自己。*

## *在这里找到我:*

*[](https://medium.com/@DrPsychick/drpsychick-on-the-web-a9ccfb0df17e) [## 网上心理咨询

### 我所在的所有网站的一个简单的“登陆页”。

medium.com](https://medium.com/@DrPsychick/drpsychick-on-the-web-a9ccfb0df17e) 

## 有关系的

[Yankee Maharjan](https://medium.com/u/348042aca5a2?source=post_page-----1521b43f7574--------------------------------) 正在使用`k3s`和`multipass`创建一个本地的多节点 K8s 集群，也非常有趣！

[](https://medium.com/@yankee.exe/setting-up-multi-node-kubernetes-cluster-with-k3s-and-multipass-d4efed47fed5) [## 使用 K3s 和 Multipass 建立多节点 Kubernetes 集群

### 有很多工具可以让您立即设置一个本地 Kubernetes 集群。但是有了全面发展的 K8s…

medium.com](https://medium.com/@yankee.exe/setting-up-multi-node-kubernetes-cluster-with-k3s-and-multipass-d4efed47fed5) 

`kind`GitHub 上的相关问题，引导我找到解决方案:
*[https://github.com/kubernetes-sigs/kind/issues/1867](https://github.com/kubernetes-sigs/kind/issues/1867)
*[https://github.com/kubernetes-sigs/kind/issues/1685](https://github.com/kubernetes-sigs/kind/issues/1685)
*[https://github.com/kubernetes-sigs/kind/pull/408](https://github.com/kubernetes-sigs/kind/pull/408)*