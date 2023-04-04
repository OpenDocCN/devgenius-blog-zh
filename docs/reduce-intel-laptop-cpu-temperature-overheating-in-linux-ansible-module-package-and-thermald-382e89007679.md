# 降低 Linux 中英特尔笔记本电脑 CPU 温度过热— ansible 模块封装和 Thermald

> 原文：<https://blog.devgenius.io/reduce-intel-laptop-cpu-temperature-overheating-in-linux-ansible-module-package-and-thermald-382e89007679?source=collection_archive---------11----------------------->

## 如何使用带封装模块的 Ansible Playbook 和零配置 Linux 热守护程序(thermald)来降低英特尔笔记本电脑 CPU 温度过热。

# Linux 下如何降低笔记本电脑 CPU 温度过热？

我将向您展示一个现场演示和一些简单的 Ansible 代码。

我是卢卡·伯尔顿，欢迎来到今天的《神秘飞行员》节目。

# 降低 Linux 中英特尔笔记本电脑 CPU 的过热温度

今天我们讨论的是开源项目“Linux thermal daemon”(`thermald`)，该项目使用最新的英特尔 sandy bridge 和最新的英特尔 CPU 版本来监控笔记本电脑和平板电脑的温度。

`thermald`工具以两种模式运行:

*   零配置模式

对于大多数用户来说，这应该足以控制系统的 CPU 温度。它使用 DTS 温度传感器，并使用英特尔 P 状态驱动程序、电源箝位驱动程序、运行平均功率限制控制和`cpufreq`作为冷却方法。

*   用户定义的配置模式

这允许在热 XML 配置文件中进行 ACPI 风格的配置。这可以用来修复漏洞百出的 ACPI 配置，或者通过添加更多的传感器和冷却设备对其进行微调。这是在用户模式下实现闭环热控制的第一步，可以根据社区反馈和建议进行改进。

它作为一个包“`thermald`”提供给今天最常用的发行版。

请注意，此服务可能会降低笔记本电脑的性能。

# Ansible 在 Linux 中安装一个包

*   `ansible.builtin.package`
*   通用操作系统软件包管理器

今天我们讨论的是 Ansible 模块包。

全名是 ansible.builtin.package，这意味着它是 ansible“内置”并附带的模块集合的一部分。

这些模块相当稳定，已经发布多年了。

它的目的是作为一个通用的操作系统包管理器。

# 因素

*   name *string — n* 名称或特定于包的名称
*   状态*字符串—* 存在/安装/不存在/移除/最新

最重要的参数是“名称”和“状态”。

在“`name`”参数中，您将指定软件包的名称或您想要安装的特定版本。

``state`'指定了我们想要执行的操作。在我们的例子中，install 是“存在或已安装”。

# 链接

*   [https://docs . ansi ble . com/ansi ble/latest/collections/ansi ble/builtin/package _ module . html](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/package_module.html)
*   [https://www.phoronix.com/scan.php?page=article&item =英特尔-thermald-tgl & num=1](https://www.phoronix.com/scan.php?page=article&item=intel-thermald-tgl&num=1)
*   [https://wiki.debian.org/thermald](https://wiki.debian.org/thermald)

# 演示

让我们跳到现实生活的剧本中，安装`thermald`包，并使用 Ansible 剧本在 Linux 中启动零配置服务。

## 密码

```
---
- name: thermald demo
  hosts: all
  become: true
  tasks:
    - name: thermald installed
      ansible.builtin.package:
        name: thermald
        state: present - name: thermald running
      ansible.builtin.service:
        name: thermald
        state: started
        enabled: true
```

## 执行

```
ansible-pilot $ ansible-playbook -i virtualmachines/demo/inventory thermald.ymlPLAY [thermald demo] **************************************************************************************TASK [Gathering Facts] ************************************************************************************
ok: [demo.example.com]TASK [thermald installed] *********************************************************************************
changed: [demo.example.com]TASK [thermald running] ***********************************************************************************
changed: [demo.example.com]PLAY RECAP ************************************************************************************************
demo.example.com           : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## 幂等性

```
ansible-pilot $ ansible-playbook -i virtualmachines/demo/inventory thermald.ymlPLAY [thermald demo] **************************************************************************************TASK [Gathering Facts] ************************************************************************************
ok: [demo.example.com]TASK [thermald installed] *********************************************************************************
ok: [demo.example.com]TASK [thermald running] ***********************************************************************************
ok: [demo.example.com]PLAY RECAP ************************************************************************************************
demo.example.com           : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## 执行前

```
[root@demo devops]# dnf list thermald
Updating Subscription Management repositories.
Available Packages
thermald.x86_64                    2.4.6-1.el8                     rhel-8-for-x86_64-appstream-rpms
[root@demo devops]#
```

## 执行后

```
[root@demo devops]# dnf list thermald
Updating Subscription Management repositories.
Installed Packages
thermald.x86_64                        2.4.6-1.el8                        [@rhel](http://twitter.com/rhel)-8-for-x86_64-appstream-rpms
```

[](https://github.com/lucab85/ansible-pilot) [## GitHub-lucab 85/ansi ble-Pilot:ansi ble Pilot YouTube 频道代码库

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/lucab85/ansible-pilot) 

# 概述

现在，您已经知道如何使用 ansible 模块包和 Thermald 在 Linux 中降低英特尔笔记本电脑 CPU 温度过热。

我希望你喜欢读这篇文章。如果你愿意支持我成为一名作家，可以考虑注册[成为一名媒体成员](https://ansiblepilot.medium.com/membership)。每月只需 5 美元，你就可以无限制地使用 Medium。

订阅 [YouTube 频道](https://www.youtube.com/channel/UC5MNbTYRHSCu9vAki3z9SmA)、 [Medium](https://ansiblepilot.medium.com/) 和[网站](https://www.ansiblepilot.com/)，不要错过 Ansible 试播的下一集。

# Ansible 的最佳资源

# 视频课程

*   [在 200 多个示例中学习 Ansible Automation&实践课程:通过一些关于如何使用最常见模块的真实示例和 ansi ble 行动手册来学习 ansi ble](https://www.udemy.com/course/ansible-by-examples-devops/?referralCode=8E065F6D6F8622A3DEC8)

# 书

*   [可通过示例回答:针对 Linux 和 Windows 系统管理员和开发人员的 200 多个自动化示例](https://leanpub.com/ansiblebyexamples)
*   [适用于 Windows 的示例:针对 Windows 系统管理员和开发人员的 30 多个自动化示例](https://www.amazon.com/dp/B09VCTP1L4)
*   [Ansible For Linux by Examples:针对 Linux 系统管理员和开发人员的 100 多个自动化示例](https://www.amazon.com/dp/B09VCX4CVD)
*   [Ansible Linux 文件系统示例:30 多个针对现代 IT 基础设施的 Linux 文件和目录操作自动化示例](https://www.amazon.com/dp/B09X4TF8P8)
*   [通过示例负责容器和 Kubernetes:10 多个自动化示例来自动化容器、Kubernetes 和 OpenShift](https://www.amazon.com/dp/B09XVDWD4W)
*   [负责安全示例:100 多个自动化示例，用于自动化安全和验证 IT 现代化基础设施的合规性](https://www.amazon.com/dp/B09VHFLSPP)
*   [可行的技巧和诀窍:10 多个可行的例子来节省时间和自动化更多的任务](https://www.amazon.com/dp/B09ZJ4GYM2)
*   [Ansible Linux 用户&按示例分组:20 多个关于现代 IT 基础设施的 Linux 用户和分组操作的自动化示例](https://www.amazon.com/dp/B09X6BYY8W)
*   [ansi ble For VMware by Examples:10 多个自动化您的 VMware 基础架构的示例(Ansible by Examples)](https://www.amazon.com/dp/B0B18QG44S)
*   [Ansible For PostgreSQL by Examples:10 多个自动化 PostgreSQL 数据库的示例](https://www.amazon.com/dp/B0B3RZZQVG)

# 捐赠

[](https://patreon.com/lucaberton) [## 卢卡·伯尔顿正在为 Ansible | Patreon 创建软件开源

### 今天就成为卢卡·伯尔顿的赞助人:获得世界上最大会员的独家内容和体验…

patreon.com](https://patreon.com/lucaberton) [](https://github.com/sponsors/lucab85) [## GitHub 赞助商上的赞助商@lucab85

### 我是一个活跃的开源贡献者，参与到了 Ansible 社区中，尽管我到处都是。@lucab85 的…

github.com](https://github.com/sponsors/lucab85) ![](img/7a9d66d627c190052e122bb050c6bf07.png)