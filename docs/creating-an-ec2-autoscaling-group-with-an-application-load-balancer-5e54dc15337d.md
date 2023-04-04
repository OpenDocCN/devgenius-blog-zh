# 使用应用程序负载平衡器创建 EC2 自动扩展组

> 原文：<https://blog.devgenius.io/creating-an-ec2-autoscaling-group-with-an-application-load-balancer-5e54dc15337d?source=collection_archive---------14----------------------->

Apache web 服务器的自动化供应和扩展

![](img/f7c0767c6cb01a7743e5b52e83daee21.png)

在本教程中，我们将为您的 Apache web 服务器创建一个自动伸缩组，并将其置于应用程序负载平衡器之后。

# 要求

*   AWS 自由层

# 步骤 1:创建 VPC

第一步是创建一个新的 VPC 来划分我们的 web 应用资源。我们将在三个不同的可用性区域中创建三个公共子网，以确保高可用性。

登录控制台后，从仪表板中找到 VPC 服务。点击**创建 VPC** 。

![](img/4cd50c4a731398476dce84d08f172e8b.png)

我们将利用 AWS 新的 ***创建 VPC 体验*** 来为我们的资源自动生成名称，并为我们创建子网、路由表和互联网网关。

让我们将我们的项目命名为" **Web-App** "并将 **10.10.0.0/16** 网络用于我们的 VPC，并将其划分为以下公共子网: **10.10.1.0/24** 、 **10.10.2.0/24** 和 **10.10.1.0/24** 。

![](img/d7abf5a8d9cca96af1237fd88dd41f28.png)

展示新的创造 VPC 体验

将可用性区域的数量更改为 3，将专用子网的数量更改为 0。使用我们指定的填充自定义的子网 CIDR 块，并将 VPC 端点更改为无。

![](img/0baa703c1397e7b2fac348d3fc6f4e2d.png)![](img/c8cc18c4d19b89822789e0333241fc0d.png)

# 步骤 2:创建启动模板

现在，我们的 VPC 和网络资源已经创建，我们可以创建一个启动模板，将用于我们的自动缩放组。

首先，让我们创建一个安全组来允许 web 流量。导航到 VPC >安全组，然后点击“创建安全组”。

![](img/8922f4669c3bdc505dc3a3c46458e115.png)

创建以下安全组，以允许来自任何来源的 TCP 端口 80 和 443 的入站流量。姑且称之为“允许-Web-访问”。在下拉列表中选择我们刚刚创建的 VPC。然后单击“创建安全组”

![](img/81b56dc8345d010fa232a98c6da1030f.png)

导航到实例>启动模板，然后单击“创建启动模板”

![](img/1e681e97bdc6b215f2026ac3d4ab68bd.png)

给出一个模板名，并从自由层和 t2.micro 实例中选择一个 Amazon AMI。

![](img/d1a8a17118087cd8b127ae0c4092e45e.png)

分配一个预先存在的密钥对和我们刚刚创建的安全组。在“高级网络配置”下，将“自动分配公共 IP”更改为启用**。**

**![](img/18bb3a32fd5397be9caec58093176caf.png)**

**点击“高级详细信息”箭头，向下滚动到“用户数据”字段。输入下面的代码，这些代码将用于在启动时提供我们的 Apache web 服务器。然后单击创建。**

```
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
EC2AZ=$(curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone) 
echo '<center><h1>This Amazon EC2 instance is located in Availability Zone: AZID </h1></center>' > /var/www/html/index.txt
sed "s/AZID/$EC2AZ/" /var/www/html/index.txt > /var/www/html/index.html
```

**![](img/e95cff80b70524c81806ce1ddfe5f36c.png)**

# **步骤 3:创建自动缩放组**

**导航到自动缩放>自动缩放组，然后单击“创建自动缩放组”**

**![](img/45eae3c59a652ed929070d5be60524ff.png)**

**给它起个名字，然后选择我们刚刚创建的模板。**

**![](img/0fc464420c14e64f74778f892e873003.png)**

**在下一页上，从下拉列表中选择我们新创建的 VPC，并选中所有 3 个子网的复选框。**

**![](img/7dc3f5d6034e901d9e9d62ee9d4d4700.png)**

**选择“连接到新的负载平衡器”选项，然后选择“面向互联网”。**

**![](img/3d24b9d1b554ac31727f1ec762275fee.png)**

**选择“监听器和路由”下的“创建目标组”,保留默认值。单击下一步。**

**![](img/927814ff67d67475f15c02e5a19cdb20.png)**

**将组大小更改为如下所示的设置，然后单击“跳过查看”，然后单击“创建自动缩放组”。**

**![](img/c8a52adb47a7f721feb95f658ce8f58d.png)**

**你现在应该看到我们的自动缩放组列出。**

**![](img/71e40814cc0510d5039f2c568194ee25.png)**

**您可以到控制台中的 EC2 实例部分进行确认。**

**![](img/9f8fe99ffe264a5ea9ef303256edf0ec.png)**

**目前，我们的 web 应用可以通过我们创建的安全组接收入站流量。但是我们的负载平衡没有应用安全组。让我们转到我们的负载平衡器并应用该组。**

**![](img/1ff31fe657294f92389fc9c8bd10dc1c.png)**

**在我们的负载平衡器配置中，转到 security 选项卡，然后单击 edit 添加我们的“Allow-Web-Access”安全组。**

**![](img/8dda4ffedd33edc771d3eb6589e95734.png)****![](img/a1d06f153960ca831c1803e8d1d961d0.png)**

**保存更改后，复制我们的负载平衡器的 DNS 名称，并将其输入到 web 浏览器中以测试我们的 web 服务器。**

**![](img/c39b00fe00e30a685b0f1520a0d31bc8.png)**

**显示负载平衡器 DNS 名称**

**![](img/c8337020d792db89dfcdeca8e6e4efd3.png)**

**显示工作的 web 服务器**

**成功！就是这样。**