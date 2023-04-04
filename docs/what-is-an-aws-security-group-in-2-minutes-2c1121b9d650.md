# 2 分钟内什么是 AWS 安全组？

> 原文：<https://blog.devgenius.io/what-is-an-aws-security-group-in-2-minutes-2c1121b9d650?source=collection_archive---------3----------------------->

![](img/6fc9dab10ec692715110f00010bc22c7.png)

照片由 [Unsplash](https://unsplash.com/photos/JX9slYyUgC0) 上的 [Flex Point Security](https://unsplash.com/@flexpointsecurity) 拍摄

关于[“您的 VPC 的安全组”](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)的 AWS 文档说*“安全组充当您的实例的虚拟防火墙，以控制入站和出站流量。”*

这是一个很好的思考方式，需要注意的是，安全组可以为流量指定*允许*或*许可*规则，但不能为流量指定*拒绝*或*限制*规则。

安全组允许您为入站(入站)流量和出站(出站)流量设置不同的规则。安全组还允许您准确指定允许的协议和端口号。例如，您可以选择`TCP`协议或`UDP`协议。你也可以像`80`或者`443`一样选择一个具体的端口号，也可以选择一个*范围*的端口号(像`8000–9000`)。

当您在 VPC 或虚拟私有云中启动 EC2 实例时，默认情况下，您最多可以为该实例分配五个安全组。这意味着 VPC 中的每个实例都可以有一组不同的安全组。

假设您在安全组中定义了以下内容:

*   **入库**
*   **来源:** 0.0.0.0/0
*   **协议:** TCP
*   **端口:** 80

这实际上意味着安全组允许来自所有 IPv4 地址的入站 HTTP 访问。

为了允许来自所有 IPv4 地址的入站 **HTTPS** 访问，然后将`port`从`80`编辑为`443`。

对于处于初级水平的安全组，需要注意的另一件有用的事情是它们是有状态的。这意味着，当您从实例发送请求时，入站安全组规则与您请求的*响应流量*无关。不管怎样，那是允许回来的。

同样，无论任何出站安全组规则如何，任何允许的入站请求的响应流量都被允许传出。记住这一点很重要。

我希望这篇 AWS 安全组的两分钟初级读本在您深入研究之前有所帮助！

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)