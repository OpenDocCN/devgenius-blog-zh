# 如何用 6 个步骤建立自己的 docker 形象

> 原文：<https://blog.devgenius.io/how-to-build-your-own-docker-image-in-6-steps-9b5e7461302?source=collection_archive---------5----------------------->

直接了当的 6 个步骤，你将拥有自己的 docker 形象

![](img/164052a9b11f3f07882c12055265a006.png)

照片由[杰克·希尔斯](https://unsplash.com/@jakehills?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/search/photos/steps?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

首先，让我们从乞讨开始..什么是 docker 图像？

> 一个 **Docker 映像**是一个文件，由多层组成，用于执行 Docker 容器中的代码。一个**映像**本质上是由一个完整的、可执行的应用程序版本的指令构建的，它依赖于主机操作系统内核。

## **1)获取 GitHub 账户和 docker hub 账户**

如果您已经在 GitHub 和 docker hub 上拥有帐户，可以跳过这一步，否则请遵循以下步骤:

== >转到[https://github.com/](https://github.com/)

并创建您的帐户—保存数据以备后用

== >转到[https://hub.docker.com/](https://hub.docker.com/)

并在 docker hub 上创建您的帐户—保存数据以备后用

## **2)创建一个新项目和 docker 文件**

创建项目的目的只是为了上传到 dockerhub，否则，你只能创建一个 dockerfile。

你想知道 dockerfile 是什么吗？

> `**Dockerfile**`是一个文本文档，包含用户可以在命令行上调用的所有命令，以组合一个图像。

你可以在这里看到一些例子:

```
#
# Super simple example of a Dockerfile
#
FROM ubuntu:latest
MAINTAINER tamara torres "tammy@gmail.com"

RUN apt-get update
RUN apt-get install -y python python-pip wget

ADD hello.py /home/hello.pyWORKDIR /home
```

—

```
#
# A bit more complex example of a Dockerfile
#
FROM ubuntu:xenialENV DEBIAN_FRONTEND noninteractiveRUN apt-get update --fix-missing && \
  apt-get install -y \
    git \
    apt-utils \
    wget \
    curl \
    build-essential \
    libssl-dev \
    openjdk-8-jre \
    xvfb \
    libgconf-2-4 \
    libexif12 \
    firefox \
    supervisor \
    netcat-traditional \
    jq \
    ffmpegRUN echo "deb [http://packages.cloud.google.com/apt](http://packages.cloud.google.com/apt) cloud-sdk-xenial main" > /etc/apt/sources.list.d/google-cloud-sdk.list
RUN curl [https://packages.cloud.google.com/apt/doc/apt-key.gpg](https://packages.cloud.google.com/apt/doc/apt-key.gpg) | apt-key add -# Set the locale (dates may be borked in protractor if not)
RUN apt-get update && apt-get install -y locales locales-all
RUN locale-gen en_GB.UTF-8
ENV LANG en_GB.UTF-8
ENV LANGUAGE en_GB:en
ENV LC_ALL en_GB.UTF-8RUN apt-get install -y google-cloud-sdkRUN curl -sL [https://deb.nodesource.com/setup_8.x](https://deb.nodesource.com/setup_8.x) | bash - && \
  apt-get update --fix-missing && \
  apt-get install -y nodejs chromium-browserRUN npm install -g protractorRUN apt-get clean && rm -rf /var/lib/apt/lists/*# Install Selenium and Chrome driver
RUN webdriver-manager update# Add a non-privileged user for running Protrator
RUN adduser --home /project --uid 1100 \
  --disabled-login --disabled-password --gecos node node# Add main configuration file
ADD supervisor.conf /etc/supervisor/supervisor.conf# Add service defintions for Xvfb, Selenium and Protractor runner
ADD supervisord/*.conf /etc/supervisor/conf.d/# Container's entry point, executing supervisord in the foreground
CMD ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisor/supervisor.conf"]# Protractor test project needs to be mounted at /project
VOLUME ["/project"]
```

## **3)编译并运行您的 docker 映像**

如果你遇到麻烦，这可能是最痛苦的一步，因为在这里你会注意到你是否正确地添加了 dockerfile 上的脚本，以及你是否忘记了依赖关系等等。

```
#build your image  
#docker build  -t {YourDockerHubUser}/{YourDockerImageNAme}:{tagVersion} path/To/dockerfiledocker build  -t "ttorres/docker-example:v1" path/To/dockerfile 
```

## 4)进入你的码头工人形象

这一步只是为了好玩(？)

事实上，最好的测试方法是在本地进行，在与其他人共享之前，在这种情况下，您只需使用 bash 进入容器即可。我强烈建议这么做，你还问为什么？如果您需要在 docker 文件中添加其他内容，或者只是确保在那里添加了必要的内容，那么使用 bash 是最简单的方法。

您在这里可以做的是运行完全相同的命令，然后(一旦您对结果满意)可以添加到 dockerfile 中。我在这里假设你正在阅读这篇文章，或者你已经有了一些关于使用 bash 控制台编写脚本的知识，或者你正在 google 它。无论哪种方式，您都可以使用以下代码进入 docker 容器上的 bash 控制台:

```
#Run the image  
#docker run {YourDockerHubUser}/{YourDockerImageNAme}:{tagVersion}
docker run ttorres/my-docker-image:tagVersion01#Query the containers running 
docker ps#Get into the container
docker exec -it {containerId} /bin/bash
```

万一你想知道这个…你能在它上面执行什么？我们可以这样想，是一个 Linux 控制台(让我们假设您在 docker 文件上配置了一个 Linux)，所以记住您可以在 Linux 控制台上运行任何您可以运行的东西，下面是一些简单的例子:

```
#list what is on the folder you are 
ls -la#Download chrome and install itwget [https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb](https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb)dpkg -i google-chrome-stable_current_amd64.deb#install ping using apt-get
apt-get update
apt-get install iputils-ping
```

## 5)为您的映像创建 GitHub repo 和 docker hub

由于本文旨在关注 docker 图像，我将让专家告诉您如何[创建 repo](https://help.github.com/en/articles/create-a-repo)

同样的事情让专家告诉你如何[创建一个 docker hub repo](https://docs.docker.com/docker-hub/repos/) ，你会发现执行起来非常简单

## 6)上传您的 docker 图像

这是非常容易的一步，因为困难的工作是在前 5 步！哦，是的，你来到这里，你做了很多！如果这是你第一次上传图片，可能会有成就感:)

```
#Login to docker by console 
docker login docker.io#Query the containers running 
#docker push ttorres/my-docker-image:tagVersion01
docker push {YourDockerHubUser}/{YourDockerImageNAme}:{tagVersion}
```

现在，你完成了！！可以请别人下载看看效果如何。

# 一些链接…

[](https://docs.docker.com/engine/reference/builder/) [## 码头文件

### 本页描述了您可以在 Dockerfile 文件中使用的命令。当您阅读完本页后，请参阅…

docs.docker.com](https://docs.docker.com/engine/reference/builder/) [](https://towardsdatascience.com/learn-enough-docker-to-be-useful-b7ba70caeb4b) [## 学习足够的码头工人是有用的

### 第 1 部分:概念景观

towardsdatascience.com](https://towardsdatascience.com/learn-enough-docker-to-be-useful-b7ba70caeb4b) [](https://towardsdatascience.com/learn-enough-docker-to-be-useful-b0b44222eef5) [## 学习足够的码头工人是有用的

### 第 3 部分:一打漂亮的 Dozen 文件指令

towardsdatascience.com](https://towardsdatascience.com/learn-enough-docker-to-be-useful-b0b44222eef5) 

希望你在我写这篇文章的时候和我一样喜欢它！

下次见！！