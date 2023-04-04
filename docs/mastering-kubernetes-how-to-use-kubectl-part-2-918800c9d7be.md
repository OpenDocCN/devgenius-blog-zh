# 掌握 Kubernetes:如何使用 Kubectl 的配置第 2 部分

> 原文：<https://blog.devgenius.io/mastering-kubernetes-how-to-use-kubectl-part-2-918800c9d7be?source=collection_archive---------4----------------------->

## 如何配置 Kubetctl 工具？

![](img/949f97b763fe8a834b1ec49f026a0628.png)

# 介绍

大多数时候，我们不需要使用我们的 Kubernetes 集群，因为它会自动协调我们的工作负载。

我说“大部分时间”是因为我们需要采取以下行动:

*   部署新的工作负载
*   更新工作量
*   回滚工作负荷
*   工作负荷故障排除

为此我们有两个选择:

*   直接使用**Kubernetes API**[https://Kubernetes . io/docs/concepts/overview/Kubernetes-API/](https://kubernetes.io/docs/concepts/overview/kubernetes-api/)
*   使用工具 https://kubernetes.io/docs/reference/kubectl/**库贝克**T6

我们只使用 Kubernetes API 通过应用程序与 Kubernetes 集群进行通信，以满足特定的需求。

所以很多时候当我们需要做上面提到的某个动作的时候，我们会使用工具 **Kubectl！**

# 如何安装和使用 Kubectl？

我们有两个选择:

*   使用一个全局项目工具来创建 Kubernetes 集群，并自动安装直接指向该 Kubernetes 集群的 Kubectl:Docker Desktop、Minikube 等..
*   直接下载并安装 Kubectl。

我们将在这里看到第二种选择。

Kubectl 项目可以在以下链接中找到:

[](https://github.com/kubernetes/kubectl) [## GitHub - kubernetes/kubectl:问题跟踪器和 kubectl 代码镜像

### k8s.io/kubectl 回购用于跟踪与 k8s.io/kubernetes. It 部门一起分发的 kubectl cli 的问题…

github.com](https://github.com/kubernetes/kubectl) 

并且是用 Go 语言编写的。

要安装 Kubectl，我们可以使用下面的教程:

[](https://kubernetes.io/docs/tasks/tools/) [## 安装工具

### 在您的计算机上设置 Kubernetes 工具。Kubernetes 命令行工具 kubectl 允许您运行命令…

kubernetes.io](https://kubernetes.io/docs/tasks/tools/) 

因为我使用的是 Windows 11，所以我会按照教程进行操作

[](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/) [## 在 Windows 上安装和设置 kubectl

### 在开始之前，您必须使用与您的集群相差一个次要版本的 kubectl 版本。对于…

kubernetes.io](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/) 

我们只需要下载最新版本:

[https://dl . k8s . io/release/v 1 . 25 . 0/bin/windows/amd64/kubectl . exe](https://dl.k8s.io/release/v1.25.0/bin/windows/amd64/kubectl.exe)

然后，我们可以将 kubectl.exe 位置添加到 PATH env 变量中，这样我们就可以在命令行中使用它了。

现在我们准备配置我们的工具 Kubectl。

在没有任何配置的情况下，我们的工具 Kubectl 对 Kubernetes 集群一无所知。

这将是下一节的主题

# 如何配置 Kubectl？

让我们运行以下命令:

```
>kubectl config
Modify kubeconfig files using subcommands like "kubectl config set current-context my-context"The loading order follows these rules:1\.  If the --kubeconfig flag is set, then only that file is loaded. The flag may only be set once and no merging takes
place.
  2\.  If $KUBECONFIG environment variable is set, then it is used as a list of paths (normal path delimiting rules for
your system). These paths are merged. When a value is modified, it is modified in the file that defines the stanza. When
a value is created, it is created in the first file that exists. If no files in the chain exist, then it creates the
last file in the list.
  3\.  Otherwise, ${HOME}/.kube/config is used and no merging takes place.Available Commands:
  current-context   Display the current-context
  delete-cluster    Delete the specified cluster from the kubeconfig
  delete-context    Delete the specified context from the kubeconfig
  delete-user       Delete the specified user from the kubeconfig
  get-clusters      Display clusters defined in the kubeconfig
  get-contexts      Describe one or many contexts
  get-users         Display users defined in the kubeconfig
  rename-context    Rename a context from the kubeconfig file
  set               Set an individual value in a kubeconfig file
  set-cluster       Set a cluster entry in kubeconfig
  set-context       Set a context entry in kubeconfig
  set-credentials   Set a user entry in kubeconfig
  unset             Unset an individual value in a kubeconfig file
  use-context       Set the current-context in a kubeconfig file
  view              Display merged kubeconfig settings or a specified kubeconfig fileUsage:
  kubectl config SUBCOMMAND [options]Use "kubectl <command> --help" for more information about a given command.
Use "kubectl options" for a list of global command-line options (applies to all commands).
```

它说默认情况下使用的配置文件是:${HOME}/。kube/配置。嗯＄{ HOME }是一个 Linux 环境变量，在 Windows 上它是%userprofile%

让我们创建这个目录。kube 和文件配置:

```
> cd %userprofile
> mkdir .kube
> cd .kube
```

我们刚刚创建了。我们的用户配置文件目录中的 kube 目录。

现在我们将创建配置文件，它是 YAML 格式的 Kubeconfig 文件。

它将列出以下项目:

*   集群详细信息
*   用户
*   上下文

当我们使用 kubectl 和 config 命令时，我们只需要知道上下文。

我们可以看到以下依赖性:

上下文= >集群= >用户

一旦我们选择了一个上下文，Kubectl 将获得集群信息，以便在控制平面中访问 Kubernetes API。最后，当 Kubectl 发送命令时，Kubectl 将使用用户信息在 Kubernetes 集群上获得正确的认证和授权。

让我们用最少的信息创建我们的 config yaml 文件:一个上下文:

*   名称:上下文名称
*   名称空间:集群中的默认名称空间
*   user:在 Kubernetes API 中用于身份验证的用户名

```
apiVersion: v1
kind: Config
contexts:
- context:
    cluster: minikube
    namespace: default
    user: minikube
  name: minikube
```

我们只是添加了一个名为 minikube 的上下文，因为我使用 minikube 在我的开发机器上工作。

现在让我们运行命令:

```
>kubectl config get-contexts
CURRENT   NAME       CLUSTER   AUTHINFO   NAMESPACE
          minikube
```

很好，我们现在知道了:

*   kubectl 使用什么配置文件
*   如何创建 yaml 配置文件并添加上下文

现在我们需要知道如何从 minikube 上下文连接到我们的集群。

事实上，Kubectl 无法连接这个基本配置文件:

```
>kubectl get node
Unable to connect to the server: dial tcp [::1]:8080: connectex: No connection could be made because the target machine actively refused it.
```

我们不得不等待一段时间，因为得到这个响应，我们有一个超时。

现在让我们看看如何添加连接到 Minikube 集群的信息:

首先，我们添加集群信息:

*   服务器:找到 kubernetes API 的 URI
*   名称:集群名称
*   certificate-authority:它是用于颁发证书的机构:它的数据在%user%profile 中加密。minikube\ca.crt

它告诉使用 Kubernetes API 将使用证书来验证请求。

让我们添加集群信息:

```
apiVersion: v1
kind: Config
contexts:
- context:
    cluster: minikube
    namespace: default
    user: minikube
  name: minikube
clusters:
- cluster:
    certificate-authority: C:\Users\nbarlatier\.minikube\ca.crt
    server: [https://127.0.0.1:52213](https://127.0.0.1:52213)
  name: minikube
preferences: {}
```

让我们看看我们得到了什么:

```
>kubectl config get-contexts
CURRENT   NAME       CLUSTER    AUTHINFO   NAMESPACE
          minikube   minikube   minikube   default
```

我们取得了进步:

*   上下文名称
*   集群名称
*   AuthInfo:是用户名
*   名称空间:默认名称空间

最后，让我们使用将用于验证用户身份的用户信息:

*   名称:用户的名称
*   具有客户端证书和客户端密钥的用户用于通过 CA 颁发的证书对用户进行授权

```
apiVersion: v1
kind: Config
contexts:
- context:
    cluster: minikube
    namespace: default
    user: minikube
  name: minikube
clusters:
- cluster:
    certificate-authority: C:\Users\nbarlatier\.minikube\ca.crt
    server: [https://127.0.0.1:52213](https://127.0.0.1:52213)
  name: minikube
users:
- name: minikube
  user:
    client-certificate: C:\Users\xxx\.minikube\profiles\minikube\client.crt
    client-key: C:\Users\xxx\.minikube\profiles\minikube\client.key
```

现在我们得到:

```
>kubectl config get-contexts
CURRENT   NAME       CLUSTER    AUTHINFO   NAMESPACE
          minikube   minikube   minikube   default
```

我们只需要用命令告诉 kubectl 默认的上下文是什么:

```
>kubectl config use-context minikube
Switched to context "minikube".
```

现在我们得到了

```
>kubectl config get-contexts
CURRENT   NAME       CLUSTER    AUTHINFO   NAMESPACE
*         minikube   minikube   minikube   default
```

我们注意到 Current 下面的星号，所以现在 kubectl 可以调用 minikube 集群的 kubernetes API

让我们试试:

```
>kubectl get nodes
NAME       STATUS   ROLES                  AGE   VERSION
minikube   Ready    control-plane,master   17d   v1.23.3
```

耶，我们做到了！

现在，我们更好地了解了如何配置我们的工具 Kubectl，我们可以轻松地导入其他上下文/集群/用户信息，并将我们的 kubectl 切换到我们需要的上下文，这样我们就可以从我们的开发机器管理任何 kubernetes 集群，甚至是远程集群(如果管理员给了您正确的用户，并且显然打开了网络)！

我们可以手动添加配置文件的不同部分，或者使用 kubectl，例如使用以下命令:

```
>kubectl config set-cluster
```

向我们的集群添加信息。

我们可以使用

```
>kubectl config set-context
```

向我们的上下文添加信息

我们可以使用

```
>kubectl config set-credentials
```

当请求被发送到 Kubernetes API 时，添加用于身份验证的用户信息。

我们需要浏览参考资料，查看所有可用的选项。

但是这篇文章只是一个介绍。您可以在结论部分的参考资料中找到更多详细信息。

此外，当我们不添加参数时，例如当我们使用不带参数的 set-credentials 时，kubectl 可以帮助我们:

```
>kubectl config set-credentialsSets a user entry in kubeconfigSpecifying a name that already exists will merge new fields on top of existing values.Client-certificate flags:
  --client-certificate=certfile --client-key=keyfileBearer token flags:
    --token=bearer_tokenBasic auth flags:
    --username=basic_user --password=basic_passwordBearer token and basic auth are mutually exclusive.Examples:
  # Set only the "client-key" field on the "cluster-admin"
  # entry, without touching other values:
  kubectl config set-credentials cluster-admin --client-key=~/.kube/admin.key# Set basic auth for the "cluster-admin" entry
  kubectl config set-credentials cluster-admin --username=admin --password=uXFGweU9l35qcif# Embed client certificate data in the "cluster-admin" entry
  kubectl config set-credentials cluster-admin --client-certificate=~/.kube/admin.crt
--embed-certs=true# Enable the Google Compute Platform auth provider for the "cluster-admin" entry
  kubectl config set-credentials cluster-admin --auth-provider=gcp# Enable the OpenID Connect auth provider for the "cluster-admin" entry with additional args
  kubectl config set-credentials cluster-admin --auth-provider=oidc
--auth-provider-arg=client-id=foo --auth-provider-arg=client-secret=bar# Remove the "client-secret" config value for the OpenID Connect auth provider for the
"cluster-admin" entry
  kubectl config set-credentials cluster-admin --auth-provider=oidc
--auth-provider-arg=client-secret-# Enable new exec auth plugin for the "cluster-admin" entry
  kubectl config set-credentials cluster-admin --exec-command=/path/to/the/executable
--exec-api-version=client.authentication.k8s.io/v1beta1# Define new exec auth plugin args for the "cluster-admin" entry
  kubectl config set-credentials cluster-admin --exec-arg=arg1 --exec-arg=arg2# Create or update exec auth plugin environment variables for the "cluster-admin" entry
  kubectl config set-credentials cluster-admin --exec-env=key1=val1 --exec-env=key2=val2# Remove exec auth plugin environment variables for the "cluster-admin" entry
  kubectl config set-credentials cluster-admin --exec-env=var-to-remove-Options:
      --auth-provider='': Auth provider for the user entry in kubeconfig
      --auth-provider-arg=[]: 'key=value' arguments for the auth provider
      --embed-certs=false: Embed client cert/key for the user entry in kubeconfig
      --exec-api-version='': API version of the exec credential plugin for the user entry in
kubeconfig
      --exec-arg=[]: New arguments for the exec credential plugin command for the user entry in
kubeconfig
      --exec-command='': Command for the exec credential plugin for the user entry in kubeconfig
      --exec-env=[]: 'key=value' environment values for the exec credential pluginUsage:
  kubectl config set-credentials NAME [--client-certificate=path/to/certfile]
[--client-key=path/to/keyfile] [--token=bearer_token] [--username=basic_user]
[--password=basic_password] [--auth-provider=provider_name] [--auth-provider-arg=key=value]
[--exec-command=exec_command] [--exec-api-version=exec_api_version] [--exec-arg=arg]
[--exec-env=key=value] [options]Use "kubectl options" for a list of global command-line options (applies to all commands).
error: Unexpected args: []
```

# 结论

我们可以找到关于 kube 配置的所有参考资料:

[](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/) [## 使用 kubeconfig 文件组织集群访问

### 使用 kubeconfig 文件来组织关于集群、用户、名称空间和认证机制的信息。的…

kubernetes.io](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/)  [## Kubectl 参考文档

### 本节包含在集群上运行工作负载的最基本命令。运行将开始运行 1…

kubernetes.io](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#config) [](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/) [## 配置对多个集群的访问

### 本页显示如何使用配置文件配置对多个集群的访问。在您的集群之后，用户…

kubernetes.io](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/)