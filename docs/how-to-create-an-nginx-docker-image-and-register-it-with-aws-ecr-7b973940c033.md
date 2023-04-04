# 如何创建 NGINX Docker 映像并将其注册到 AWS ECR

> 原文：<https://blog.devgenius.io/how-to-create-an-nginx-docker-image-and-register-it-with-aws-ecr-7b973940c033?source=collection_archive---------4----------------------->

![](img/6ef20f8622358d284e4196d66330aba3.png)

Docker 是什么？Docker 是一款开源的容器化软件，用于开发、部署和管理应用程序。Docker 使开发人员能够将配置文件、库和应用程序打包到一个隔离的环境中。

在本教程中，您将…

1.  使用 Nginx 创建您自己的映像，并添加一个文件来告诉您部署容器的时间
2.  在端口 8080 打开的情况下部署容器
3.  将容器数据保存到弹性容器注册表中

> ***先决条件:*** AWS 帐号(ECR 访问)
> [DockerHub](https://hub.docker.com/) 帐号
> 安装[Docker](https://docs.docker.com/engine/install/)安装 AWS CLI
> Vim

我将使用 CentOS 8。

# 步骤 1:用 Vim 创建 Dockerfile

在您登录到您的服务器并安装 Docker 之后，您需要将这两个文件保存在终端的当前目录中:

最后，您希望用这两个命令启动 docker:

![](img/3a9e9f3aec8709dbdd39e6fdf0fc87f3.png)![](img/6ffb0268fd657ec90c1e15588aecc955.png)

# 步骤 2:构建 Docker 映像

通过输入以下命令构建 Docker 映像:

![](img/655bc50e2b4e72f2ca39fe725785c151.png)![](img/6aeadf8194f813a999b42b05432d7e83.png)![](img/5ad77ae19611440bada54b18151a71be.png)

# 步骤 3:部署端口 8080 打开的容器

使用 Run 命令下拉 Nginx 映像，并在端口 8080 上本地打开容器:

![](img/8a82952689be6a72051bee40acf99add.png)![](img/87d6074b7152f5f535a478c4ec34c3f5.png)

# 步骤 4:将容器数据保存到弹性容器注册中心

1.  通过输入以下命令在终端中安装 AWS CLI:

![](img/fbbdbf4947b2390a9e011b59e66000b3.png)

2.通过输入以下命令配置 AWS CLI:

![](img/7efd52ed567b18ed938a9daac0b2daa9.png)

按照提示输入您的 AWS 访问密钥 ID、AWS 秘密访问密钥和您的默认区域名称。

3.输入以下命令以获取 AWS 帐户 ID 号。

![](img/690b754489c76e119b1910857fff7cd4.png)![](img/558aa0d7bf71577d1bac8f377e3d1b2f.png)

一定要复制你的“账号 ID”。我们将在以下命令中使用它来执行 ECR。

4.向 AWS 验证 Docker:

```
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin **<account_id>**.dkr.ecr.us-east-1.amazonaws.com
```

在您看到<account_id>的地方，输入您在上面记下的 ID。输入后，您应该会在底部收到一条“登录成功”的消息。</account_id>

![](img/bb1894df7a5a2a7614bf21d4c1601a90.png)

5.创建新的 ECR 存储库:

![](img/64448aa4082387f8675f78e690a629ae.png)![](img/bb96aeace2e678e9845d546f26c91ef5.png)

6.列出图像，以便您可以识别要标记和推送的图像:

![](img/9f3e7e6aff040e5440097ccc3c6e63a5.png)![](img/674e27371654b3235b80746a1e21248c.png)

7.将图像标记到存储库中:

```
docker tag **<image_id>** **<account_id>**.dkr.ecr.us-east-1.amazonaws.com/my-nginx:tag
```

![](img/277cf7bdc7e4609aa5665d11976d3edb.png)

8.推送图像:

```
docker push **<aws_account_id>**.dkr.ecr.**<region>**.amazonaws.com/my-nginx
```

![](img/e750a3051da10edea59ab37530f6dde9.png)

# 决赛:

1.  在网络浏览器中查看网页确认:

![](img/4e4906b4837f323c31f55724bdb0882a.png)

2.查看 AWS 控制台 ECR 确认:

![](img/43cbbf70a20dae42c52504cb9c8b0169.png)![](img/2d2389e47ca50cc0cd1d7dada019c326.png)

你做到了！您已经创建了映像，部署了打开了端口 8080 的容器，并且从 AWS CLI 将容器数据保存到了弹性容器注册表中！