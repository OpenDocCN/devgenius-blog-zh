# 我如何从服务器到 Jenkins 进行 ssh

> 原文：<https://blog.devgenius.io/how-i-can-make-ssh-from-server-to-jenkins-8dcc34647c6b?source=collection_archive---------1----------------------->

![](img/77630ca34d54c385cde57ca8b4cb3d5b.png)

## 简介:

在本文中，我将解释如何通过 ssh 命令行从 Jenkins 服务器登录到另一个服务器

我们可以在 Jenkins 中创建新的凭据，以使用不同的 **IP 地址**访问另一台服务器来部署项目，但不能使用传统方式的用户名和密码创建凭据。不，当我想使用不同的 IP 地址访问另一台服务器时，这是不正确的。解决方案是为用户创建公钥和私钥，并将它们作为机器注入 Jenkins 和 Jenkins 服务器。

我建议在开始阅读本文之前先阅读这篇文章:

*   如果不知道什么是詹金斯，你可以阅读这篇文章。
*   如果您不知道什么是 CI/CD 概念，请阅读本文。
*   如果你不知道什么是詹金斯管道阅读这篇文章。
*   如果你不知道如何运行詹金斯作为一个 docker 图像阅读这篇文章。

我可以从 Jenkins 直接登录到我们的服务器，因为这不安全，但 Jenkins 会用他的方式在您的服务器中部署您的项目

在本文中，我想解释如何使用管道在我们的服务器内部部署我的项目。

> *你一定知道* [*什么是 CI/CD 概念？*](/what-is-ci-cd-concept-375cb226cf3d) *，* [*詹金斯是什么？*](https://medium.com/@abdalrhmanalkraien/what-is-jenkins-bc8bebcc05cf) *，* [*CI/CD 概念用管道*](https://medium.com/@abdalrhmanalkraien/ci-cd-concept-with-pipeline-3408d2fff38)T22[*什么是 Jenkins 管道和 JenkinsFile？*](/what-is-jenkins-pipeline-and-jenkinsfile-96f30f3a29c) *为理解本文*

# 让我们来处理它:

我有 Jenkins 作为 docker 镜像运行在本地机器上，我有**云服务器**。

我将把我的项目部署到我的**云服务器**上，但是从**本地机器**使用 **CI/CD 概念使用 Jenkins 管道**。

在这种情况下，我们遇到了一个问题，因为 Jenkins 无法使用用户名和密码在云服务器内部进行部署。

在这篇文章中，我将解决这个问题。让詹金斯登录我的云服务器。

我们将在这里应用另一种安全方法，称为 **ssh 安全密钥**，这种方法依赖于两个文件，称为**公钥**和**私钥**

将把来自云服务器的**公钥文件**放入本地机器中，并将来自云服务器的**私钥**放入 Jenkins 凭证中，以制作新凭证。

> *我正在使用 Ubuntu 20.4LTS 作为云服务器的操作系统，我想在操作系统中制作* ***ssh 安全密钥*** *，windows 有不同的方法来制作 ssh 安全代码*

*   第一步，我将从我的终端登录云服务器:

在这一步中，我可以使用下面的命令来访问我的云服务器

```
ssh -l root my-ip-address
```

在这里，我将通过 **root** 用户登录我的云服务器。按 enter 键后，您必须输入正确的**密码**才能登录您的服务器。

*   好的，现在我会写一些命令，如果你需要的话，但是我认为在这篇文章中你不需要这些

下面的命令可以看出现在登录的是什么用户

```
whoami
```

*   你一定会看到这样的结果

```
root@ubuntu-16gb-hel1-1:~# whoamiroot
```

*   如果我需要查看什么是 IP 地址或其他重要信息，我可以编写以下命令

```
iconfig
```

或者相当于

```
ip addr show
```

*   如果您想创建一个新用户，您可以使用以下命令

```
sudo adduser newUserName
```

在上面的命令中，您必须编写密码和其他信息来创建用户

*   好了，现在我想在我的服务器上创建 shh key，下面的命令将创建 shh key

```
ssh-keygen
```

上述命令将在**内生成两个文件的私钥和公钥。操作系统中的 ssh** 目录

`ssh-keygen`使用 **SHA256** 算法公开您的安全密钥。

按回车键后你可以看到如下输出

```
Generating public/private rsa key pair.Enter file in which to save the key (/root/.ssh/id_rsa):Enter passphrase (empty for no passphrase):Enter same passphrase again:Your identification has been saved in /root/.ssh/id_rsaYour public key has been saved in /root/.ssh/id_rsa.pubThe key fingerprint is:SHA256:n8/vqeJTUa8AUsDat4gsuluxaWnRTbN4UjOgMacwJ+o root@abed-Inspiron-5593The key's randomart image is:+---[RSA 3072]----+|  + + +..o.      || . = - .+     .  ||.   o  =*+   . . ||.    .o=*=  o    || E  o.+oS    o   ||   ..= o . . ..  ||    =.o   o.     ||   ... .  o    . ||    ..   ..o++   |+----[SHA256]-----+
```

*   现在我想转到公钥和私钥目录，我可以使用以下命令转到目录

```
cd /root/.ssh/
```

*   之后如果我想看什么？/ssh 文件具有我可以编写的以下命令。

```
ls
```

*   我可以在按下回车键后看到这个结果

```
id_rsa  id_rsa.pub  known_hosts
```

在上述结果中，我可以找到 3 个文件，我只需要上述文件中的两个文件

`id_rsa`它是一个私钥

`id_rsa.pub`它是一个公钥

*   在这一步中，我将把公钥复制到我的本地服务器，这是同一个 Jenkins 服务器

```
ssh-copy-id -i id_rsa.pub user@ip.address.here
```

*   现在，如果你想从本地服务器登录到云服务器，你可以写这个命令，你可以注意云服务器不希望从你写云服务器密码。

```
ssh user@ip.address.here
```

**为什么现在云服务器不需要写密码？还有云服务器怎么保证你有没有权限？**

因为 has 本地机器有一个公钥，而云服务器有私钥，在这种情况下，当你想登录并提取密码并检查是否匹配时，私钥将解密公钥。

*   好了，现在我们想从云服务器中提取我们的私钥，我们可以使用这个命令来提取它，

```
cat id_rsa
```

*   按回车键后，可以看到这个结果

```
-----BEGIN OPENSSH PRIVATE KEY-----b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAABlwAAAAdzc2gtcnNhAAAAAwEAAQAAAYEAxVsChxM8CXeb6ZmoOA1OpGMrLSzDHoCqinsdmanGnGNNHQ0+j8q+JVKvc9EZxkjfmVDYPUp1VF99bc9ZrZUDzOspmpl/3Vgu8mW2ZDxVoEqJzLkDS5ZK4rQpYLOgw7+LWA8EGtgpFmIVfPssG79itFiNF4PPJWbzoUSk0dRW3v8vS/joApkuvTOyis8viKUyTbT7Dii1eEhRklQVPj+g02u2nK0baHDBqoZfyDJTmA/X4EuxPW5wieojM23Ze0Nf9Gkv+jvGDW+jsP6FFKaMA2VmeEAPeyfsx1YqqBh/T7MES8bYrXUnhfCDcj60MzrLSxgUbmUPDxi6Ts/pk3X8Rt4Ms+mjYmps8SodAXZ8g2D007T2xcQPTmxGGuJf6gi7AvTdv9Uc7AWfU7uqpJzDmtWy+ECqhOqetpPKweMR0t4pf79bkVe/eqIVFpNBKOqH/23NwnKrGoaTvtfI5syWg7JPOL3eVr4Q2ZEPorXYmW22cvEujBBXyq29YUdZcm+fAAAFkBtGxtYbRsbWAAAAB3NzaC1yc2EAAAGBAMVbAocTPAl3m+mZqDgNTqRjKy0swx6Aqop7HZmpxpxjTR0NPo/KviVSr3PRGcZI35lQ2D1KdVRffW3PWa2VA8zrKZqZf91YLvJltmQ8VaBKicy5A0uWSuK0KWCzoMO/i1gPBBrYKRZiFXz7LBu/YrRYjReDzyVm86FEpNHUVt7/L0v46AKZLr0zsorPL4ilMk20+w4otXhI//// other code here-----END OPENSSH PRIVATE KEY-----
```

*   我们将复制该密钥，并将其放入 Jenkins 凭据中。
*   现在，我们将转到**Jenkins UI**查看以下图片以了解步骤:
*   -打开 Jenkins 仪表板时，我们将转到管理 Jenkins

![](img/f59062fe044b9be8fe9f58a13aa2c31d.png)

*   表格**管理詹金斯**我们将前往**管理凭证**

![](img/f5abd0049425e37095416601bc498c0e.png)

*   从**管理凭证**我们将创建新凭证

![](img/e0b713b9d3418f66f66c281c34ecbe30.png)

上图中是一个创建新凭据

*   我选择了一种叫做**SSH 用户名的凭证，带有私钥
*   我为这个名为 **Login_Cloud_Server** 的凭证写了 Id，我将在 JenkinsFile 中使用它
*   在我的例子中选择用户名是**根**
*   最后，我在 Jenkins 内部放了一个云服务器的私钥

按“确定”后，我们将拥有一个名为 **Login_Cloud_Server** 的新凭据

您可以看到下面的图像

按下“确定”后，我们将拥有名为**登录 _ 云 _ 服务器**的新凭据

您可以看到下面的图像

![](img/61691da082b03c50b4f14b5687003fb1.png)

## 我如何在詹金斯文件中使用它？

```
pipeline{ agent any stages{ stage('login server'){ steps{
            sshagent(credentials:['Login_Cloud_Server']){ sh 'ssh  -o StrictHostKeyChecking=no  root@135.181.203.3 uptime "whoami"' } echo "success lgoin" } } }}
```

运行管道后，您必须在输出中看到

```
.....root // from serversuccess lgoin // from pipeline stage.....
```

# 结论

在本文中，我解释了如何通过私钥创建新的 ssh 凭证。

当您有两台服务器时，您可以使用这种方式第一台服务器负责 Jenkins 管理，第二台服务器负责您的项目。

而且两个服务器对它没有任何依赖。

如果你想应用 **CI/CD 概念**并且在你的项目中使用 docker 和 kubernetes，这种方式是非常重要的。

你可以看这篇文章了解什么是 **CI/CD 概念**，你也可以看这篇文章了解什么是 **Jenkins** 作为一个项目

# 参考

https://medium . com/edu reka/what-is-continuous-integration-40 e 75 e 8326 e 8

[https://jenkins-x.io/about/concepts/](https://jenkins-x.io/about/concepts/)