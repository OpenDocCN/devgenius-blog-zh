# 不要像 BFU 一样使用 Docker

> 原文：<https://blog.devgenius.io/stop-using-docker-like-a-bfu-229215e6e06d?source=collection_archive---------25----------------------->

![](img/0786cd4b8c29946b48d24ad0f17b9a05.png)

在与各种客户打交道的过程中，我遇到了很多使用 docker 的开发人员——甚至是资深开发人员，他们没有任何更深入的知识或问题。运行其他人提供的命令，而不知道它们实际上是为什么和做什么的。

如果您是其中一员，或者您只是想对这项技术了解更多一点，那么这篇文章就是为您准备的。所以让我们开始吧。

# Dockerfile 文件

**Dockerfile** 不是某个天才 DevOps 的家伙准备的黑匣子。 **Dockerfile** 通常是一个非常简单的文件(如果做得对的话),描述你的应用程序将在其中运行的容器。

当您构建一个**docker 文件**时，您将获得 **docker 映像**。然后，Docker 映像可以被推送到**映像注册中心，例如 [Docker Hub](https://hub.docker.com/) 、 [Gitlab 注册中心](https://about.gitlab.com/)或其他私有或公共注册中心提供者，并用作其他 Docker 文件的基础映像。**

大多数 docker 文件都包含来自、 **COPY** 、 **RUN、**、 **WORKDIR** 、 **ENTRYPOINT** 和 **EXPOSE** 的命令，对于每个至少具备基本 unix 命令知识的开发人员来说，它们的意义应该非常明显。

**来自**定义父 docker 镜像，通常带有预安装的运行时环境。所以对于 Javascript 的 NodeJs，对于 Java 的 Gradle 或 OpenJDK，对于 PHP 的 PHP-FPM 等等，你懂了吧。

**COPY** 顾名思义就是从容器内部的本地文件系统中复制文件或目录。复制文件有时可以通过**添加**完成，其功能与**复制**相似，但也有其他特定用途。欲了解更多信息，请查阅[官方文档](https://docs.docker.com/engine/reference/builder/#add)。

**运行**将执行任何提供的命令。很简单。

**WORKDIR** 为下面的命令定义了根目录，所以你不必每次都指定完整的路径。在提到的例子中，**复制**命令实际上将文件复制到 */app* 目录，而**运行**将执行 */app* 目录中的命令。第二个**复制**命令将保持原样，因为它使用整个系统路径。

**入口点**将在容器开始时执行提供的命令或可执行文件。在特定情况下，可以使用 **CMD** 代替**入口点**。如果你想了解更多，我再次建议你查阅官方文档。

**EXPOSE** 定义容器监听的默认公开端口。

# Docker 撰写

Docker compose，也就是您应用程序中神秘的**Docker-composer . yml**文件(很久以前由其他人准备的)，定义了 docker 服务的配置，这些服务可以作为整个堆栈在您的本地环境中运行。

基本 docker 组合文件包含配置文件的**版本**和**服务的列表**。

## 图像或构建

每个服务或者定义了 docker **映像**或者属性 **build** 指向 **Dockerfile** 文件所在的文件夹。如果 **Dockerfile** 位于其他地方或者具有不同的名称，则 **build** 属性可能看起来像:

```
build:
  context: ./my-folder
  dockerfile: Dockerfile-awesome
```

用**上下文**描述 Dockerfile 的路径，用 **dockerfile** 描述其名称。

> **构建**或**上下文**路径也为 **Dockerfile** 命令定义了根文件夹。

## 港口

公开容器端口，以便可以从本地主机访问它们。

```
ports:
  - localPort:containerPort
```

## 卷

您可以将容器目录或文件挂载到本地磁盘。大多数情况下，当您想要持久化数据，以便在重新创建容器时不会丢失数据，或者在开发过程中想要使用某种热重新加载时，您会用到这个特性。

```
volumes:
  - local-path:container-path
```

在我的例子中，我挂载了 *src/* 和 *config/* 文件夹，所以这些目录中的项目文件和容器文件会自动同步。

## 环境

包含传递给运行容器的 ENV 变量列表。

> 注意 **mongo-db** 服务在变量 **DB_URL** 中被引用。默认情况下，docker 服务相互公开，只需在 URL 或其他变量中使用它们的名称就可以引用它们。

# 命令

## docker-向上组合(-d)

*   这可能是创建和启动堆栈的最重要的命令。
*   如果您的堆栈已经在运行，并且您已经更新了 **docker-compose.yml** ，它将重新创建受更新影响的服务。
*   使用参数 **-d** 或 **- detach** 将在后台运行堆栈。

> **Mac 版 Docker**和【Windows 版 Docker 带有预装的 docker **仪表盘** UI，你可以看到所有正在运行的容器或整个 Docker 用日志和其他有用的信息组成堆栈。

## docker-撰写停止

*   停止当前 docker 合成堆栈中的所有容器

## docker-向下合成

*   除了停止所有容器之外，还会删除所有容器以及所有相关的网络、卷和映像。

## docker-compose build(SERVICE _ NAME)(-no-cache)

*   通过不带任何参数运行该命令，docker compose 将使用 **build** 属性重建所有服务(使用本地 Dockerfile 的服务)
*   通过指定服务名，docker compose 将只重建指定的服务
*   参数 **-无缓存**将在不使用缓存的情况下重建 docker 映像(默认情况下**运行**命令层被缓存，而**复制**或**添加**不被缓存)

## docker-撰写拉取(服务名称)

*   如果没有任何参数，docker compose 将尝试使用指定的**图像**来获取所有服务的最新图像
*   如果您在开发期间使用 docker 图像标签，如**最新**、**开发**或 **lts** ，这个命令会非常有用
*   只有当图像作者推送具有完全相同版本号的新图像时，具有特定版本(如 3.2.1)的图像才会被更新
*   使用服务名作为参数，它将只尝试提取服务映像

## 结论

关于 docker 和 docker compose、其他 Dockerfile 命令和 docker compose 属性，肯定还有很多东西需要了解和学习，但这是我根据自己的经验认为绝对基础的东西。

希望它能帮助你或给你指出正确的方向，如果是这样，请留下👏或者很少。