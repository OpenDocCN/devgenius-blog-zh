# 在 Heroku 上部署 dockerized 闪亮的应用程序

> 原文：<https://blog.devgenius.io/deploying-dockerized-shiny-apps-on-heroku-892b96f010ca?source=collection_archive---------9----------------------->

![](img/af8061cb3858b459b454cd542cd72c3a.png)

照片由 unsplash 拍摄

Heroku 是一个平台即服务(PaaS ),使开发人员能够完全在云中构建、运行和操作应用程序。他们提供免费服务，你最多可以运行 5 个应用程序，但如果你用信用卡验证你的帐户，你可以运行 5 个以上的应用程序。

*注意:我假设您已经创建了您的应用程序，并且在应用程序目录的根目录下有它的 dockerfile。否则，如果你不介意的话，可以看看我之前写的介绍使用 shiny 创建仪表板的文章和介绍 docker 的文章。*

进入[注册](https://signup.heroku.com/)页面，创建一个账户，然后按照下面的说明操作。

## 安装 Heroku CLI

适用于 ubuntu 16 +版本

```
$ sudo snap install --classic heroku
```

对于 mac

```
$ brew tap heroku/brew && brew install heroku
```

对于 windows

参见[文档](https://devcenter.heroku.com/articles/heroku-cli#install-the-heroku-cli)

## 登录 Heroku

*不适用如果您错过了第一步，您需要先创建一个帐户。*点击[此处](https://signup.heroku.com/)并按照说明操作即可。

通过终端导航到您闪亮的应用程序目录所在的位置，并输入以下命令。

```
$ heroku login 
```

这将在您的浏览器上打开登录页面，输入所需的详细信息，如果成功，您将看到一个 ***登录…完成*** ，然后* ***作为您的电子邮件登录。***

## 创建 Heroku 应用程序

以下命令将在 Heroku 服务器上创建一个 Heroku 应用程序。

```
$ heroku create
```

## 部署代码

登录 heroku 容器注册表

```
$ heroku container:login
```

*注:*

*确保冒号前后没有空格。
你需要在电脑上安装 docker。*

## 建立形象

导航到您的项目目录，然后运行下面的命令。
这将构建图像并将其推送到容器注册中心。你需要在包含你的应用程序代码的目录下有一个 docker 文件。

```
$ heroku container:push web
```

## 推送现有图像

或者，您可以使用以下命令将现有映像推送到 heroku 注册表:

```
$ docker tag yourimage registry.heroku.com/<app>/<process-type>   
$ docker push registry.heroku.com/<app>/process-type>
```

上面的命令会将映像推送到 heroku registry，就像推送到 docker hub 一样。

## 将图像发布到您的应用程序

```
$ heroku container:release web
```

上述命令将把图像发布到 heroku 服务器上的应用程序中。

## 我现在如何查看我的申请？

使用下面的命令可以看到应用程序并与之交互。

```
$ heroku open
```

## 要禁用您的应用程序

如果你想停止你的应用程序，只需运行下面的命令。

```
$ heroku ps:scale web=0
```

## 重启一下怎么样？

```
$ heroku ps:scale web=1
```

一些简单快捷的步骤就可以让你的闪亮应用免费部署了！

为了进一步阅读，我推荐你查看 heroku 上的文档。

亲爱的编码快乐！