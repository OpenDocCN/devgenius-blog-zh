# Windows 上的温柔集装箱指南

> 原文：<https://blog.devgenius.io/gentle-containerd-on-windows-guide-for-you-50d4a46daccf?source=collection_archive---------0----------------------->

![](img/0417e722e59b53039be303107f4a2ce1.png)

哈马·哈基在 [Unsplash](https://unsplash.com/s/photos/container-window?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

H 你有没有想过 Windows containerd 可以作为一个独立的服务来使用？最近发现 Windows Server 2022 发布了，ContainerD 也在今年达到了 Windows 上普遍可用的状态。

这篇文章将展示在没有 Docker Enterprise 的独立 Windows 服务器上安装和启动你自己的 Windows 容器的详细步骤。然后，您可以根据本指南来配置和理解具有多个 Windows worker 节点的混合 Kubernetes 集群。

# 背景

如你所知，米兰蒂斯收购了 Docker Enterprise 部门，他们将 Docker Enterprise 更新为米兰蒂斯容器运行时。与此同时，Kubernetes 社区决定使用 Dockershim，所以他们不会支持 Dockershim 作为集群中的主要容器运行时。最终，这些变化影响了现有的 Windows 容器生态系统。

如果愿意，您可以将现有的 Docker 企业配置迁移到 Mirantis 容器运行时。但是在大多数情况下，人们会更喜欢基于 Kubernetes 的工作负载，而不是其他类型的容器编排平台。这种情况使得 ContainerD 取代了 Windows Server 容器主机中的 Docker Enterprise。

# 先决条件

请确保在测试本文之前准备好以下项目。

*   您应该使用 Windows Server 2022 LTSC。
*   至少需要 4gb 或更多系统内存。
*   至少需要 16GiB 或更多的磁盘。
*   您已经登录到本地管理员组。

# 配置容器 d

首先，让我们激活与容器相关的特性。在此步骤中，您的服务器可能会重新启动，因此请保存您的数据并关闭您的应用。

```
$RestartRequired = (Add-WindowsFeature -Name Containers)if ($RestartRequired.RestartNeeded) { Restart-Computer -Force }
```

然后，下载最新版本的 ContainerD。

```
$ContainerDVersion = '1.5.5'
curl.exe -LO "[https://github.com/containerd/containerd/releases/download/v${ContainerDVersion}/cri-containerd-cni-${ContainerDVersion}-windows-amd64.tar.gz](https://github.com/containerd/containerd/releases/download/v${ContainerDVersion}/cri-containerd-cni-${ContainerDVersion}-windows-amd64.tar.gz)"
tar.exe xvzf ".\cri-containerd-cni-${ContainerDVersion}-windows-amd64.tar.gz"
del ".\cri-containerd-cni-${ContainerDVersion}-windows-amd64.tar.gz"
```

将存档文件解压到程序文件目录中，制作 CNI 插件目录布局，并使其适合 ContainerD。

```
mkdir -force "$env:ProgramFiles\containerd"
mv .\*.exe "$env:ProgramFiles\containerd"
mv .\cni "$env:ProgramFiles\containerd"
mkdir "$env:ProgramFiles\containerd\cni\bin" -force
mv "$env:ProgramFiles\containerd\cni\*.exe" "$env:ProgramFiles\containerd\cni\bin"
```

制作容器的默认配置文件。幸运的是，ContainerD 二进制文件可以用默认值生成它自己描述的整个配置文件，所以我们可以只呈现和放置它。

```
& "$env:ProgramFiles\containerd\containerd.exe" config default | Out-File "$env:ProgramFiles\containerd\config.toml" -Encoding ascii
```

然后，用记事本或您喜欢的文本编辑器打开配置文件。我们需要更新配置文件来纠正暂停图像引用。由于 Windows 主机的不兼容暂停映像，低于 3.5 版将导致 pod 初始化失败。

```
...
[plugins][plugins."io.containerd.gc.v1.scheduler"]
    deletion_threshold = 0
    mutation_threshold = 100
    pause_threshold = 0.02
    schedule_delay = "0s"
    startup_delay = "100ms"[plugins."io.containerd.grpc.v1.cri"]
    disable_apparmor = false
    disable_cgroup = false
    disable_hugetlb_controller = false
    disable_proc_mount = false
    disable_tcp_service = true
    enable_selinux = false
    enable_tls_streaming = false
    ignore_image_defined_volumes = false
    max_concurrent_downloads = 3
    max_container_log_line_size = 16384
    netns_mounts_under_state_dir = false
    restrict_oom_score_adj = false
    sandbox_image = "k8s.gcr.io/pause:3.6" # <- Change the tag version from 3.5 to higher version (I recommend 3.6)
...
```

让我们为 Windows Defender 添加一个例外。Windows 容器依赖于内核的 API，就像 Linux 一样，Windows Defender 通过挂接设备驱动程序的调用来扫描所有文件内容。这使得容器的 I/O 性能非常糟糕，所以我们需要进行平滑的配置。

```
Add-MpPreference -ExclusionProcess "$env:ProgramFiles\containerd\containerd.exe"
```

最后，我们可以注册 ContainerD 服务并启动它。

```
& "$env:ProgramFiles\containerd\containerd.exe" --register-serviceStart-Service containerd
```

# 配置 HNS NAT 网络

众所周知，Windows 提供了自己的容器网络基础设施，称为 HNS(主机网络服务)。ContainerD 与网络特性解耦，这些特性被归入不同的插件类型，称为 CNI。

本指南将选择内置的 NAT CNI 插件，并使 NAT CNI 插件正确工作。

Windows 服务器中没有内置的 HNS 控制命令。所以我们需要下载 HNS PowerShell 模块并导入。

```
curl.exe -LO '[https://raw.githubusercontent.com/microsoft/SDN/master/Kubernetes/windows/hns.psm1'](https://raw.githubusercontent.com/microsoft/SDN/master/Kubernetes/windows/hns.psm1')
Import-Module .\hns.psm1
```

然后，让我们定义一个虚拟网络子网并注册它。

```
$subnet='10.0.0.0/16'
$gateway='10.0.0.1'
New-HnsNetwork -Type NAT -AddressPrefix $subnet -Gateway $gateway -Name "nat"
```

最后，在创建新的 pod 之前，重写 NAT 插件配置文件。CNI 是一个独立的插件，与 ContainerD 的运行状态无关，所以我们只需放入配置文件，无需重启 ContainerD 服务。

```
@"
{
    "cniVersion": "0.2.0",
    "name": "nat",
    "type": "nat",
    "master": "Ethernet",
    "ipam": {
        "subnet": "$subnet",
        "routes": [
            {
                "gateway": "$gateway"
            }
        ]
    },
    "capabilities": {
        "portMappings": true,
        "dns": true
    }
}
"@ | Set-Content "$env:ProgramFiles\containerd\cni\conf\0-containerd-nat.conf" -Force
```

# 配置 CRICTL 客户端工具

要访问 ContainerD 并将其与 CRI(容器运行时接口)合并，我们需要 CRICTL 客户端工具。CTR 工具也是可用的，但是这个工具不使用 CRI，所以用 CTR 创建的容器可能没有附加网络接口。

首先，从 GitHub 下载 CRICTL 工具。

```
$CriCtlVersion = '1.22.0'
curl.exe -LO "[https://github.com/kubernetes-sigs/cri-tools/releases/download/v${CriCtlVersion}/crictl-v${CriCtlVersion}-windows-amd64.tar.gz](https://github.com/kubernetes-sigs/cri-tools/releases/download/v${CriCtlVersion}/crictl-v${CriCtlVersion}-windows-amd64.tar.gz)"
tar.exe xvzf ".\crictl-v${CriCtlVersion}-windows-amd64.tar.gz"
del ".\crictl-v${CriCtlVersion}-windows-amd64.tar.gz"
```

然后，在您的用户配置文件目录中创建一个 CRICTL 配置文件。在 Windows 中，ContainerD 将通过命名管道而不是套接字打开控制通道。

```
mkdir -Force "$home\.crictl"@"
runtime-endpoint: npipe://./pipe/containerd-containerd
image-endpoint: npipe://./pipe/containerd-containerd
timeout: 10
"@ | Set-Content "$home\.crictl\crictl.yaml" -Force
```

您可以使用下面的命令测试配置文件。

```
.\crictl.exe info
```

# 最后一步:创建并运行 IIS 容器

漫长的旅程到此结束！如果您遵循了之前的所有步骤，现在您可以创建并运行 IIS 容器了。

开始之前，我们需要准备一些工件来创建一个 pod 和一个容器。

首先，创建一个保存日志的目录。

```
mkdir -Force \logs
```

然后，定义一个 pod 配置文件。

```
@"
metadata:
  attempt: 1
  name: sample-sandbox
  namespace: default
  uid: $((New-Guid).ToString("N").Substring(0, 25)).log
log_directory: /logs
windows:
  namespaces:
    options: {}
"@ | Set-Content "pod-config.yaml" -Force
```

将创建的 pod 配置文件传递给 CRICTL。该工具将通过 CRI 与 ContainerD 通信，并返回创建的 pod id。

```
$PodId = (.\crictl.exe runp --runtime runhcs-wcow-process .\pod-config.yaml)
.\crictl.exe pods $PodId
```

提取 IIS 映像。

```
.\crictl.exe pull mcr.microsoft.com/windows/servercore/iis:windowsservercore-ltsc2022
```

然后，创建一个容器配置文件。

```
@"
metadata:
  name: simple-server
image:
  image: mcr.microsoft.com/windows/servercore/iis:windowsservercore-ltsc2022
log_path: $((New-Guid).ToString("N").Substring(0, 8)).log
ports:
  containerPort: 80
"@ | Set-Content "simple-server.yaml" -Force
```

最后，将创建的容器配置文件传递给 CRICTL 工具，并获取容器 ID。

```
$ContainerId = (.\crictl.exe create $PodId .\simple-server.yaml .\pod-config.yaml)
.\crictl.exe start $ContainerId
```

要检查 IP 地址，请运行以下命令。

```
.\crictl.exe exec -i $ContainerId ipconfig
curl.exe '<IPAddressOfContainer>'
```

因此，您可能会发现 IIS 容器返回其欢迎页面代码。

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "[http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd](http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd)">
<html ae kc" href="http://www.w3.org/1999/xhtml" rel="noopener ugc nofollow" target="_blank">http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
<title>IIS Windows Server</title>
<style type="text/css">
<!--
body {
        color:#000000;
        background-color:#0072C6;
        margin:0;
}#container {
        margin-left:auto;
        margin-right:auto;
        text-align:center;
        }a img {
        border:none;
}-->
</style>
</head>
<body>
<div id="container">
<a href="[http://go.microsoft.com/fwlink/?linkid=66138&amp;clcid=0x409](http://go.microsoft.com/fwlink/?linkid=66138&amp;clcid=0x409)"><img src="iisstart.png" alt="IIS" width="960" height="600" /></a>
</div>
</body>
</html>
```

# 一些警告

可悲的是，大多数与 ContainerD 相关的项目严重依赖 buildkit(尤其是 buildkitd)。然而，buildkitd 不是为 Windows 容器生态系统开发的。因此，您现在无法在这种配置下构建自己的 Windows 容器映像。

# 结论

ContainerD 迟早会广泛地出现在 Windows 容器生态系统中。与 Linux 相比，Windows ContainerD 使用最新的 HCS v2 API 来支持更多等效的容器功能。(例如，在 HCS v2 中，您可以挂载单个文件，而不是一个目录。)

此外，微软决定他们不会发布半年一次的渠道发布，而只保留几年的长期服务渠道。这一决定将为 DevOps 工程师和现有的 Windows 应用程序开发人员提供一个更加稳定、直观和简单的容器平台。

因此，我认为有必要研究一下这个新的 Windows 容器平台，使您的传统 Windows 服务器应用程序现代化。

# 参考

*   [https://www . jamesturtevant . com/posts/Windows-Containers-on-Windows-10-without-Docker-using-container d/](https://www.jamessturtevant.com/posts/Windows-Containers-on-Windows-10-without-Docker-using-Containerd/)
*   [https://kubernetes . io/docs/tasks/debug-application-cluster/crictl/](https://kubernetes.io/docs/tasks/debug-application-cluster/crictl/)
*   [https://Jay unit 100 . blogspot . com/2020/12/debugging-container d-on-windows . html？视图=马赛克](https://jayunit100.blogspot.com/2020/12/debugging-containerd-on-windows.html?view=mosaic)
*   [https://kubernetes . io/ko/docs/setup/production-environment/windows/user-guide-windows-containers/](https://kubernetes.io/ko/docs/setup/production-environment/windows/user-guide-windows-containers/)