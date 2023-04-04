# 初学者必读:IT 自动化入门

> 原文：<https://blog.devgenius.io/ansible-for-beginners-get-started-with-it-automation-670f0d852268?source=collection_archive---------4----------------------->

![](img/0e49db4bafe828a1c20f00c7ce93a8fc.png)

Ansible 是一个简单、强大、无代理的工具，它简化了 IT 自动化流程，加快了开发工作。Ansible 致力于帮助您自动化和配置您的基础设施，以节省时间和提高生产力。它简单明了、安全且功能强大，使其成为您组织内易于学习和实施的工具。今天，我们将讨论什么是 Ansible，它做什么，关键术语，以及如何开始。

**我们将介绍:**

*   什么是 Ansible？
*   Ansible for DevOps 的优势
*   Ansible 的常见用例
*   关键可回答术语
*   Ansible 入门
*   你学习的下一步

# 什么是 Ansible？

Ansible 是一个**开源编排和自动化工具**，用于**软件供应、配置管理和应用部署**。Ansible 最早是在 2012 年由 Cobbler 和 Func 的创造者 Michael DeHaan 开发的。资助 Ansible 的公司于 2015 年被 RedHat，Inc .收购。RedHat 在 2019 年被 IBM 收购，所以现在，Ansible 生活在 IBM 的保护伞下。

Ansible 运行在 Windows 和类似 Unix 的操作系统上，以代码的形式提供基础设施。它有自己的声明式编程语言用于管理和系统配置。它可以**连接像亚马逊 AWS 和微软 Azure 这样的云环境**，帮助你管理和自动化你的基础设施和代码部署。

Ansible 安装简单，很容易连接到客户端，并且包含许多功能。它是基于推的，通过 SSH 连接到客户端，所以它不需要客户端上的代理。通过将模块推送到客户机，客户机上的模块在本地执行，输出被推回到 Ansible 服务器。它使用 SSH-Keys 来简化连接客户端的过程。主机名、IP 地址和 SSH 端口存储在清单文件中。一旦创建并填充了库存文件，Ansible 就可以使用它了。

# Ansible for DevOps 的优势

Ansible 是 DevOps 组织的首选工具，因为它**简化了自动化和灵活性**。一些主要优势包括:

*   开源，免费使用
*   安装和使用不需要特殊的系统管理技能
*   高度可定制
*   一致且轻便
*   由于无代理功能和 OpenSSH 安全性的使用，非常安全
*   全面的文档
*   平滑学习曲线
*   使用 Python 构建，Python 是最快、最健壮的编程语言之一
*   版本控制和配置管理
*   可靠的部署

# Ansible 的常见用例

Ansible 是一个功能强大的工具，有广泛的应用和用途，特别是将其连接到 Docker 环境或像 AWS 这样的云服务。Ansible 的一些主要用例包括:

*   结构管理
*   应用程序开发和部署
*   软件和基础设施
*   IT、安全和网络自动化
*   批量管理虚拟机，以确保每个虚拟机具有相同的配置
*   定义您在云中运行的服务器的配置，以便其他人可以轻松地阅读和使用它
*   使用 Ansible 塔或 AWX 创建一个图形用户界面

# 关键可回答术语

既然我们已经讨论了 Ansible 的好处和常见用例，那么让我们熟悉一些 Ansible 的关键术语。

*   **Ansible 服务器:**安装了 Ansible 的机器，运行所有的任务和剧本
*   **主机:**你用 Ansible 管理的设备
*   **剧本:**一个定义可行自动化任务的框架(在 YAML 编写)
*   **剧本:**剧本的执行
*   **模块:**在客户端执行的一个或一组命令
*   **任务:**包含要执行的单个程序的部分
*   **标签:**可以分配给任务的名称
*   **处理程序:**只有在通知程序存在时才调用的任务
*   **通知程序:**分配给一个任务的部分，如果输出改变，它调用一个处理程序
*   **清单:**包含可解析的客户端-服务器数据的文件
*   **事实:**使用收集事实操作从客户端的全局变量中检索到的信息

# Ansible 入门

现在是时候了解该工具的一些基础知识了。我们将带您开始使用 Ansible、Ansible 行动手册和 Ansible 特别命令。

请记住，使用 Ansible，您有一个 Ansible 服务器和主机。Ansible 服务器是安装 Ansible 的机器，主机是 Ansible 通过 Ansible 服务器处理的机器。Ansible 服务器**可以处理多个主机**。

**可行的服务器要求:**

*   Python 2 (2.7)或 3 (3.5 或更高版本)
*   Ansible 不支持 Windows 作为控制节点，但是你可以使用 WSL 在 Windows 10 上设置它

**主机要求:**

*   Python 2 (2.7)或 3 (3.5 或更高版本)

> ***注:*** *设置您的 Ansible 环境的方式取决于您设备的要求。下一步是下载 Ansible，然后配置和自动化您的体验。*

# 翻译剧本

行动手册是 Ansible 的配置管理和协调自动化的基础。您可以在其中创建指令，以确定要在不同主机中执行的任务。

剧本是**可重用的和可重复的**，使得多次执行一个任务变得容易。剧本是用 YAML 语写的，并且有少量的语法。每个剧本由一个或多个剧本组成。

每部戏至少定义两件事:

*   要执行的一个或多个任务
*   您要定位的主机

一个游戏从定义`hosts`行开始，这是一个或多个主机模式或由冒号分隔的组的列表。`hosts`行后面是一个`tasks`列表。Ansible 将在指定的主机上依次执行任务，一次一个。

例如，看看下面的行动手册:

```
---
- hosts: webservers
       tasks:
       - name: deploy code to webservers
               deployment:
                       path: {{ filepath }}
                       state: present
- hosts: dbserver
       tasks:
       - name: update database schema
         updatedbschema:
               host: {{ dbhost }}
               state: present
- hosts: webservers
       tasks:
       - name: check app status page
               deployment:
                       statuspathurl: {{ url }}
                       state: present
```

在这个剧本示例中，我们执行了三个剧本:我们将代码部署到 web 服务器，更新数据库模式，并检查应用程序状态页面。

# 临时命令

临时命令是 Ansible 环境的另一个重要部分。它们用于在一台或多台主机上自动执行单个任务。

它们提供了一种快速简单的自动化方法，但是它们并不意味着可重用性。临时命令对于临时任务非常有用。例如，如果您想在多台主机上重新启动一项服务，您可以使用一个特别的命令轻松实现。

**特殊命令的使用案例:**

*   连通性测试
*   事实收集
*   管理文件和/或服务
*   重启服务器

**不同的临时命令类型及其作用:**

*   `file`:添加和/或删除目录
*   `ping`:测试连通性
*   `stat`:检索关于目录的事实
*   `copy`:复制文件
*   `replace`:更新文件
*   `debug`:调试变量和表达式
*   `lookup`:从外部资源访问数据的插件

下面是一个`stat`命令的例子，它检索关于目录的事实:

```
ansible localhost -m stat -a "path=/ansible"
```

# 你学习的下一步

祝贺您迈出 Ansible 的第一步！这个流行的工具非常适合现代 DevOps 环境，对于现有的开发人员来说也很容易学习。现在，您可以更深入地研究 Ansible，了解更多关于以下主题的信息:

*   易变库存
*   可变角色
*   可折叠集装箱
*   更多

*快乐学习！*