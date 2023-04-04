# 将您的消息直接从云发布/订阅写到 BigQuery

> 原文：<https://blog.devgenius.io/write-your-messages-directly-from-cloud-pub-sub-to-bigquery-ece76c2b676a?source=collection_archive---------5----------------------->

最近，谷歌云宣布了新的云发布/订阅选项(新的交付类型)。通过这种方式，我们能够将流消息写入大查询，而不需要管道(但它仍然有其局限性)。

如果你有预加载转换或数据处理需求，谷歌仍然推荐数据流。

在开始这个项目之前，有件事我想提醒你。此交付类型目前支持两种数据类型。字符串和字节。所有目标 BigQuery 表都需要数据字段。您可以从[【大查询订阅】](https://cloud.google.com/pubsub/docs/bigquery)查看详情

![](img/9f4a7b0c8c052a1c4b0b49f1519daac5.png)

到 BigQuery 的云发布/订阅

# **场景**

在这个项目中，我们有这样一幅图像。

![](img/166e9449e60c8dfc44455474bf425e8b.png)

我们将转换图像 base64 格式，然后发布到发布/订阅主题。然后，我们将它直接登陆到 BigQuery，中间不使用任何其他服务。

稍后我们将到达 BigQuery 表，在查询数据和解码数据(图像的字节格式)之后，我们将到达原始图像。所以让我们更深入地研究一下。

# 启用发布/订阅服务

首先，我们需要启用发布/订阅服务。您可以遵循此路径并启用发布/订阅服务。

API 和服务>库>搜索(发布/订阅) >选择>云发布/订阅 API

![](img/e6fe23a946cfd1a0cfb3196be6a62771.png)

启用发布/订阅服务

# 创建发布/订阅主题

我们这样创建发布/订阅主题。发布/订阅>主题

![](img/0927e80da1b22d7225a678e4b85452f3.png)

创建发布/订阅主题

# 创建订阅

在上一步中，我们创建了一个订阅，在订阅中，我们收听我们创建的主题的传入消息。为此，我们将遵循以下步骤。

![](img/3dc3fd702365dd906ca61511016183c3.png)

创建订阅

***我们这里有个问题。*** 问题表示我们无法写入选定的 BigQuery 表。要解决这个问题，请遵循以下步骤。

## **1。选择数据集，然后点击共享**

![](img/a7538cba182b34c5ec361972cd245795.png)

## 2.添加新主体

![](img/29c6d9068d2cb0139fd6d8a99c2b2001.png)

## 3.添加我们想要访问的服务帐户。

![](img/17a57c6286c18fd054b123d96f538f2e.png)

这些设置现在对我们来说刚刚好，我们可以在这些步骤之后创建订阅。

# **创建发布/订阅服务证书密钥**

我们需要证书密钥来发布和访问发布/订阅主题。为此，请遵循以下步骤。

## 正在创建服务帐户

![](img/cfda9cbe0602958eb18b2dcb54fa24ff.png)

## 设置服务帐户详细信息

![](img/3432cbcb2d22461fd01116ea6f69f309.png)

## 设置服务帐户权限

![](img/caaa94881c1443e3246eeedd945b98e1.png)

## 最终访问凭证密钥

![](img/e330c05bed8effc181154b1f2aa5f8c5.png)

# 将消息发布到发布/订阅主题

我有一个如下的 python 代码。这段代码为发布/订阅主题创建了一个发布者客户端。然后我们再将图片 **base64** 格式( ***编码*** )。
而且我们会发布 base64 格式的数据到题目里。

如果一切正常，让我们运行代码。

**检查数据到达**

***TA-DAA！*** 。base64(编码)格式的数据已成功到达。

![](img/310d7575621a464e7eb3dfbbbbebed95.png)

## 从 BigQuery 获取数据并访问图像

在从 BigQuery 获取数据之前，我们需要访问(**为 BigQuery 创建新凭证**)我前面提到的 BigQuery 凭证(发布/订阅凭证)。

下面我只展示角色分配部分，其余都一样。

![](img/252be7b7c023d6952f19356c49ceadde.png)

在这里的代码中，在我们创建了一个 BigQuery 客户端并查询了相关的表之后，我们再次 ***解码(将数据 base64 转换为原始图像)*** 查询到的数据。

然后运行代码。

是的，我们成功地做到了。 [🥳](https://emojipedia.org/partying-face/) 🎉🎈

![](img/0c79360c84ef0ebaa1349adcfa2a5b5a.png)

在其他疯狂的项目中再见。祝你今天开心！玩的开心！

## Github 资源库:

[](https://github.com/TugrulGokce/Cloud-Pub-Sub-To-BigQuery) [## GitHub-TugrulGokce/Cloud-Pub-Sub-To-big query

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/TugrulGokce/Cloud-Pub-Sub-To-BigQuery)