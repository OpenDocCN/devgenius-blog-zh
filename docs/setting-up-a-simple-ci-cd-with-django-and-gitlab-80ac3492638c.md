# 使用 Django 和 Gitlab 设置简单的 CI/CD

> 原文：<https://blog.devgenius.io/setting-up-a-simple-ci-cd-with-django-and-gitlab-80ac3492638c?source=collection_archive---------2----------------------->

![](img/b053a27089d4b781702fb6d8cfe0a612.png)

我想在我的项目中实践拥有一个类似 DevOps 的环境，因此，我决定修补 CI/CD 管道，并最终与世界共享它。我希望这篇指南能帮助你建立一个简单的管道，同时揭开 DevOps 及其相关工具的神秘面纱。

# **项目概述:**

这个项目是关于有一个 Django API，并将其添加到 Gitlab repo，同时确保它作为 Docker 容器部署。因此，要做到这一点，我们必须安装这些工具，这正是下一部分的内容。然后，我们将配置这些工具来帮助实现我们的自动化管道。

请注意，我使用的环境是 Ubuntu 20.04。

# 安装 Docker:

据 https://docs.docker.com/engine/install/ubuntu/[:](https://docs.docker.com/engine/install/ubuntu/)

```
#### prerequisites
$ sudo apt-get update
$ sudo apt-get install ca-certificates curl gnupg lsb-release#### setup docker apt repository
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpgecho \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null#### install docker packages
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io#### test your installation
sudo docker run hello-world
```

# 安装 Gitlab 社区版(自管理安装):

根据[https://about.gitlab.com/install/#ubuntu](https://about.gitlab.com/install/#ubuntu):

```
## prerequisites
sudo apt-get update
sudo apt-get install -y curl openssh-server ca-certificates tzdata perl 
sudo apt-get install -y postfix## add Gitlab apt repository 
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash
```

除非您在安装过程中提供了自定义密码，否则会随机生成一个密码，并在`/etc/gitlab/initial_root_password`中存储 24 小时。使用用户名为`root`的密码登录。
另外，请注意，要使用您的服务器 IP 访问 Gitlab，您必须将 EXTERNAL_URL 设置为您想要的 IP 或 FQDN(例如:“http://192.168.1.140”)

```
sudo EXTERNAL_URL="https://gitlab.example.com" apt-get install gitlab-c
```

# 安装 Gitlab Runner:

据[https://docs.gitlab.com/runner/install/linux-manually.html](https://docs.gitlab.com/runner/install/linux-manually.html):

```
curl -LJO "[https://gitlab-runner-downloads.s3.amazonaws.com/latest/deb/gitlab-runner_amd64.deb](https://gitlab-runner-downloads.s3.amazonaws.com/latest/deb/gitlab-runner_${arch}.deb)sudo dpkg -i gitlab-runner_amd64.deb
```

确保 Gitlab-runner 正在运行:

```
sudo gitlab-runner status
```

# 从 Github 获取 Django 代码:

我已经准备了一个基于 [Dennis Ivy 优秀而简洁的教程](https://www.youtube.com/watch?v=cJveiktaOSQ&list=PLYynv4DCCc8er26ZPaXOfCUMpqIwJGV71&index=35&t=422s)的 Django API 示例:

```
cd /home/$USER
git clone [https://github.com/VertigoX64/django_gitlab_cicd](https://github.com/VertigoX64/django_gitlab_cicd)
```

现在我们有了所有需要的工具。我们只需要把代码放入 Gitlab 并启用它的内部 CICD 管道。

# 将 Django 代码输入 Gitlab:

1-登录到您为 EXTERNAL_URL 变量输入的地址。(例如:“http://192.168.1.140”)

2-创建新的存储库。

3-进入刚刚为 Django 代码克隆了 git 存储库的文件夹，运行以下命令:

```
git remote set-url origin [http://192.168.100.140/YOUR_USERNAME/YOUR_REPOSITORY_NAME.git](http://192.168.100.140/YOUR_USERNAME/YOUR_REPOSITORY_NAME.git)git push origin
```

为了确保代码已经被成功地推送到 Gitlab，请查看存储库页面。你应该看看里面的文件。

# 注册并配置 Gitlab 运行程序:

根据[https://docs.gitlab.com/runner/register/index.html#linux](https://docs.gitlab.com/runner/register/index.html#linux):

运行此命令

```
sudo gitlab-runner register
```

然后打开 Gitlab 实例，进入里面的 Django 代码 repo。打开左侧边栏上的设置菜单，并转到 CI/CD 部分。然后，展开 Runners 部分并找到注册令牌。然后，运行以下代码:

```
REGISTRATION_TOKEN=YOUR_REGISTRATIONTOKEN
sudo gitlab-runner register --url http://192.168.100.140/ --registration-token $REGISTRATION_TOKEN
```

最后一步，打开 Gitlab-runner 的配置文件(位于/etc/gitlab-runner/config.toml)并将 executor 参数的值更改为“shell”(根据下面的配置，**第 11 行**):

为了记录，这里是我用过的 gitlab-ci.yaml:

# 结束了

您可以导航到 Gitlab 侧边栏上的 CI/CD 菜单来查看您的管道。在您的管道历史记录中，应该至少有一个通过或失败的管道。

我希望这本指南会有用。