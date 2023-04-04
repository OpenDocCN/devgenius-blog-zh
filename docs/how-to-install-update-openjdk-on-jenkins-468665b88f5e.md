# 如何在 Jenkins 上安装/更新 OpenJDK

> 原文：<https://blog.devgenius.io/how-to-install-update-openjdk-on-jenkins-468665b88f5e?source=collection_archive---------4----------------------->

在 Windows 上使用 MSI 安装程序安装 OpenJDK 11

此过程描述了如何使用基于 MSI 的安装程序安装 OpenJDK 11 for Windows。

![](img/2fdf65449e209a5a6d4c2a676e6e85e9.png)

图片来源:[medium.com/the-crazy-coder](https://medium.com/the-crazy-coder/jenkins-series-1-step-by-step-guides-to-install-on-windows-de85dbffdd15)

# 获取 OpenJDK

从以下任意位置下载所需的 OpenJDK MSI 版本:

*   [https://docs.microsoft.com/en-us/java/openjdk/download](https://docs.microsoft.com/en-us/java/openjdk/download)
*   [https://adoptium.net](https://adoptium.net/temurin/archive?version=11)
*   [https://adoptopenjdk.net](https://adoptopenjdk.net/archive.html?variant=openjdk11&jvmVariant=hotspot_)→我推荐詹金斯从这里下载 JDK

# 程序

1.  以管理员身份运行 OpenJDK 11 的 MSI 安装程序。
2.  点击欢迎屏幕上的`Next`。
3.  勾选，`I accept the terms in license agreement`，然后点击`Next`。
4.  点击`Next`。
5.  接受默认值或查看[可选属性](https://access.redhat.com/documentation/en-us/openjdk/11/html/installing_and_using_openjdk_11_for_windows/msi-based-installer-properties)。
6.  点击`Install`。
7.  点击`Do you want to allow this app to make changes on your device?`上的`Yes`。
8.  验证 OpenJDK 11 for Windows 是否已成功安装，在命令提示符下运行`java --version`命令，您必须获得以下输出:

```
openjdk version “11.0.3-redhat” 2019–04–16 LTS OpenJDK Runtime Environment 18.9 (build 11.0.3-redhat+7-LTS) OpenJDK 64-Bit Server VM 18.9 (build 11.0.3-redhat+7-LTS, mixed mode)
```

# 更新 Jenkins 配置

*   转到 Jenkins 安装文件夹
*   在桌面上复制一份`jenkins.xml`文件
*   打开命令提示符，找到 JDK 的安装位置

> 如何找到 JDK 的安装位置:

如果您使用的是 Linux/Unix/Mac OS X，请尝试以下方法:

```
$ which java
```

应该输出准确的位置。之后，您可以自己设置 JAVA_HOME 环境变量。

如果您使用的是 Windows，请运行以下命令:

```
c:\> for %i in (java.exe) do @echo.   %~$PATH:i
```

应该输出准确的位置。

*   打开您在桌面上制作的`jenkins.xml`文件的副本
*   并使用 JDK 的安装路径编辑 java 路径。可能是这样的:

*   停止詹金斯服务
*   复制新的 jenkins.xml 文件→覆盖或替换现有文件
*   启动 Jenkins 服务

登录到您的 Jenkins 实例，查看“管理 Jenkins”菜单下的“*系统信息*

> 如果你喜欢这篇文章，你可能也会喜欢； [Cron & logrotate:如何使用 Cron 自动化任务](/cron-logrotate-how-to-use-cron-to-automate-tasks-a93069d9185c)

干杯！！