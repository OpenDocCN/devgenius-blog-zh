# 在 EC2 实例上安装 Jenkins 并构建您的第一个项目——初学者指南

> 原文：<https://blog.devgenius.io/installing-jenkins-on-an-ec2-instance-and-building-your-first-project-a-beginners-guide-7921108a69bc?source=collection_archive---------1----------------------->

![](img/3ccc70e6e9539807da32d27eb66ef84f.png)

**照片来源:Avi 代码**

在本文中，我将记录我是如何在 EC2 实例上安装 Jenkins 并在 Jenkins 上创建我的第一个项目的

据[**詹金斯**](https://www.jenkins.io/) **，**詹金斯是一款开源的自动化服务器，能够让全世界的开发者可靠地构建、测试和部署他们的软件。

根据 [**TechTarget**](https://searchaws.techtarget.com/definition/Amazon-EC2-instances#:~:text=An%20Amazon%20EC2%20instance%20is,Web%20Services%20(AWS)%20infrastructure.) 的说法，亚马逊 EC2 实例是**亚马逊弹性计算云(EC2)中的虚拟服务器，用于在亚马逊 Web 服务(AWS)基础设施上运行应用程序。**

我将带您完成在一个 **EC2 实例上安装 Jenkins 的过程。**

你需要的工具:

*   访问互联网
*   亚马逊账户
*   端口为 8080 的安全组已打开
*   Jenkins 安装程序
*   Java SDK 11
*   耐心

**步骤 1:设置 EC2 实例:-**

登录 AWS 账户，点击页面右上角的**服务**菜单，会出现下拉菜单，选择**计算**会弹出一个子菜单，选择 **EC2** ，如下图所示。这将带您进入 **EC2** 实例页面。

![](img/2d6ecdb15d346ea00400864b50d35fbc.png)

**亚马逊控制台**

继续**启动实例**。

![](img/35d62055d6719ffdeec224a6aa08b96b.png)

**启动实例**

一旦我们进入实例页面，第一步是选择 Amazon 机器映像(AMI)，这是设置 EC2 实例的第一步。选择自由层**亚马逊 Linux 2 AMI (HVM) —内核 5.10，SSD 卷类型。**

![](img/9b90a443ac5b4b75570850679bc57b5a.png)

然后，第二步是选择实例类型，确保选中标记为“Free Tier Eligible”的框，现在单击屏幕右下角的**Next:Configure Instance Details**按钮。

![](img/1715754568588cb4d7f7b9005dce5d43.png)

第三步**“配置实例细节”**将保留默认设置，并继续下一步。

![](img/867de4f2ffda9b31bbdf6450e1aa8f09.png)

第四步是**“添加存储”**，也将保持默认设置不变。继续点击页面右下角的**添加标签**按钮，进入下一步。

![](img/ded54efeac839d19ec318e2c0f7ff46b.png)

第五步是**“添加标签”，**我们将向实例添加一个标签。点击**键**选项正下方的**添加标签**按钮。

![](img/ea750c1ef243c016ce88f712d28d9db5.png)

在**添加标签**页面上，将空字段标记如下 **Key:** Name， **Value:** Jenkins_Server。点击**下一步:配置安全组**

![](img/fda648fb68f2cf3affbc7c69ea83a9bb.png)

第六步**【配置安全组】，**点击**添加规则。**

![](img/57a8f40a01a36a36e8a8e513ddc73b0f.png)

现在，将**协议**设置为 **TCP** ，将**端口**设置为 **8080，**点击**查看并启动。**

![](img/f1626839ebb8e5aba40fb0ccd801508a.png)

查看并启动 **EC2 实例。**

![](img/ccca2656b1443cd179657e333f324e05.png)

单击启动实例按钮。

![](img/27ad38785cfa7ef693b70625b46c719b.png)

选择现有的密钥对或创建新的密钥对。

![](img/5c2fcb8d5849257a15e52bb84bb0abca.png)

如上图所示创建一个新的密钥对并启动。

![](img/96a79e5cbcd0110d75b51788d96586e1.png)

检查启动状态。

![](img/96ba6904fe44fbc5e6366070abeb30f4.png)

向下滚动以查看实例。

![](img/36f932875c99e9e291019ac6d0277b22.png)

现在，该实例已经启动并正在运行。我们将链接(SSH)到我们的 EC2 实例中。

**链接(SSH)到已经创建的 EC2 实例**

首先，从 EC2 实例复制公共 IP 地址。然后在本地计算机上的终端上运行下面的代码，使用 SSH 登录到 EC2 实例:

$ `ssh -i launch.pem ec2-user@3.84.253.47`

![](img/89d25cec1ea7e1e0101d0244cbe69dac.png)

然后使用以下命令安装 Java:

`$ which amazon-linux-extras`

![](img/cf35c9c011d4c146a8125d2b8eab5aa4.png)

`$ amazon-linux-extras`

![](img/104851eb800064a46e3951e833dfa240.png)

`$ sudo amazon-linux-extras enable java-openjdk11`

![](img/e84d5c9c1e9dea11a742f72c7b980da8.png)

```
[ec2-user ~]$ sudo yum clean metadata && sudo yum install java-openjdk11
```

![](img/956f33402106e17acb5c0e3438301e01.png)

现在使用下面的命令将 Jenkins 添加到存储库中:

```
[ec2-user ~]$ sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
```

![](img/a5821888fc008cefea83d2aff906aa81.png)

添加 Jenkins CI 中的密钥文件，以便从软件包中启用 Jenkins 安装:

```
[ec2-user ~]$ sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
```

![](img/6c152a0bcb61a4c35f52c2093314be57.png)

```
[ec2-user ~]$ sudo amazon-linux-extras install epel
```

![](img/08e569a99a39882e8a6d4e75f7d40754.png)

**安装詹金斯:**

```
[ec2-user ~]$ sudo yum install jenkins
```

![](img/5a45d9979ee2e4bf2f2c100f73330005.png)

要使 Jenkins 服务在引导时启动，请将 Jenkins 作为服务启动并检查状态:

```
[ec2-user ~]$ sudo systemctl enable jenkins[ec2-user ~]$ sudo systemctl start jenkins[ec2-user ~]$ sudo systemctl status jenkins
```

![](img/6b9b671d9a7d5cb94619036bc9236713.png)

# **配置詹金斯**

Jenkins 现已安装并运行，请继续配置 Jenkins。为此，请使用浏览器通过管理控制台连接到 Jenkins:

回到您的亚马逊(AWS)帐户，复制公共 IPv4 DNS 地址:

![](img/f408ff53c01d23942b19fd8dd977bbc6.png)

**公共 IPv4 DNS 地址**

`http://<your_server_public_DNS_address>:8080`

将地址粘贴到您最喜欢的浏览器上，然后启动

```
http://ec2-18-205-237-18.compute-1.amazonaws.com:8080
```

这将打开 Jenkins 配置页面，帮助您入门。

键入以下命令在终端上显示初始密码:

```
[ec2-user ~]$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

![](img/868539f438e175b857d2933d3fc5c823.png)

**初始密码显示命令**

执行命令后，复制显示的字母数字键:

![](img/98a2d0275d4b0a54075530c901f3a252.png)

**初始密码显示**

将从您的终端复制的密码粘贴到 Jenkins 配置页面上提供的字段中，然后单击继续。

![](img/3780e3e65ca8f069f4d1aeddb7ead5d5.png)

**詹金斯配置页面**

在“入门”页面上，单击“安装建议的插件”来初始化所需的插件，以帮助执行项目。

![](img/3c76f41f9cc929285abc2ce80cea7f28.png)

**入门**

安装将运行一段时间，之后，它们将加载 Jenkins 管理控制台。

![](img/622c5e9926d7908e2aaff54b8d54dacf.png)

**插件安装**

安装完成后，您会看到下面的提示。创建第一个管理员用户。

![](img/78016e9e4551c981de5fa2ac89018bf8.png)

**创建第一个管理员**

![](img/8b6593af7f143da7f8b77c9bd93b276a.png)

**入门**

登录到控制台后，配置 Java 路径:

单击控制台页面左侧的“管理 Jenkins”。

![](img/43307fc70f200615739dc6b2f5e9d659.png)

**詹金斯仪表盘**

单击全局工具配置按钮。

![](img/a38306ceb845c5411223b46b2c5b0aab.png)

**管理詹金斯**

**添加 JDK** → **添加 Java 路径** → **取消勾选自动安装框并保存**。

![](img/28c997c43a4bf6bbfa2a4a7ab7bd691a.png)

**全局工具配置**

构建您的第一个样本项目

从**仪表盘**点击仪表盘右上角的**新项目**按钮。

![](img/17def6821d7b54e23f6625e914e553d3.png)

**仪表盘**

为项目输入一个**名称**如“我的开发之旅”，选择 F**re style 项目**并点击 **OK** 。

![](img/d59daa6af483d864186705ec669e33c0.png)

**项目**

从窗格中选择**构建环境**，向下滚动到**添加构建步骤**选择**执行 shell。**

![](img/63abcd77c3d09a863a857dd3e9e94cec.png)

**构建环境**

在**执行 shell** 命令框中输入该命令:**回显“欢迎来到我的 DevOps 之旅”**，点击保存。

![](img/c788759effaa28a54ce62ecfe8bd1ae8.png)

**执行外壳**

要检查项目的状态，选择立即构建→构建历史→ #1 →控制台。

![](img/9faeb4a0507137175835e1249616f40f.png)

**项目状态**

![](img/01a471ae8870c1930fcb1c00d81c39ec.png)

**控制台输出**

**恭喜你！！！！您已经成功构建了您的第一个 Jenkins 项目。**

Yaaaaaaay！！！！我们做到了！！！

现在，请记住，这篇文章不仅是给软件领域的专家看的，即使是新手也可以从中学习到很多东西，这就是为什么我试图用外行和专业的术语把一切都讲清楚，所以如果您有任何问题，可以通过[**Twitter**](http://www.twitter.com/anh01x)**联系我，或者通过**[**GitHub**](https://github.com/uerosuo)**找到我。**

**感谢阅读❤️**

如果你对这个话题有任何想法，请留下评论——我乐于学习和探索知识。

**点击订阅按钮，了解更多关于 DevOps 和云的内容。**