# 可行的故障排除—无法通过 ssh 主机本地主机端口 22 连接到主机

> 原文：<https://blog.devgenius.io/ansible-troubleshooting-failed-to-connect-to-the-host-via-ssh-host-localhost-port-22-6fe34278ab67?source=collection_archive---------0----------------------->

## 如何解决使用 ansible_connection 本地参数在本地机器上测试代码时，通过 ssh 主机 localhost 端口 22 连接时出现连接失败致命错误的错误？

今天我们将讨论 Ansible 故障排除，特别是关于通过 ssh 连接到主机失败的本地主机错误。我是卢卡·伯尔顿，欢迎来到今天的《神秘飞行员》节目。

# 链接

*   [https://docs . ansi ble . com/ansi ble/latest/user _ guide/playbooks _ delegation . html](https://docs.ansible.com/ansible/latest/user_guide/playbooks_delegation.html)

# 演示

Ansible 连接失败问题的现场演示，并在**库存**文件中修复。

终端上的确切错误是:

```
fatal: [localhost]: UNREACHABLE! => {"changed": false, "msg": "Failed to connect to the host via ssh: ssh: connect to host localhost port 22: Connection refused", "unreachable": true}
```

## 错误代码

*   ping.yml

```
---
- name: ping module demo
  hosts: all
  tasks:
    - name: test connection
      ansible.builtin.ping:
```

*   存货

```
localhost
```

## 错误执行

```
ansible-playbook -i inventory ping.ymlPLAY [ping module demo] *****************************************************************TASK [Gathering Facts] ******************************************************************
fatal: [localhost]: UNREACHABLE! => {"changed": false, "msg": "Failed to connect to the host via ssh: ssh: connect to host localhost port 22: Connection refused", "unreachable": true}PLAY RECAP ******************************************************************************
localhost                  : ok=0    changed=0    unreachable=1    failed=0    skipped=0    rescued=0    ignored=0
```

## 修复代码

*   ping.yml

```
---
- name: ping module demo
  hosts: all
  tasks:
    - name: test connection
      ansible.builtin.ping:
```

*   存货

```
localhost ansible_connection=local
```

## 修复执行

```
$ ansible-playbook -i inventory ping.ymlPLAY [ping module demo] *****************************************************************TASK [Gathering Facts] ******************************************************************
[WARNING]: Platform darwin on host localhost is using the discovered Python interpreter
at /opt/homebrew/bin/python3.10, but future installation of another Python interpreter
could change the meaning of that path. See [https://docs.ansible.com/ansible-](https://docs.ansible.com/ansible-)
core/2.13/reference_appendices/interpreter_discovery.html for more information.
ok: [localhost]TASK [test connection] ******************************************************************
ok: [localhost]PLAY RECAP ******************************************************************************
localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

# 概述

现在，您更好地了解了如何排除最常见的关于无法通过 ssh 主机 localhost 端口 22 连接到主机的错误。

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

### 我是一个活跃的开源贡献者，参与到了 Ansible 社区中，尽管我到处都是。1 个赞助商有…

github.com](https://github.com/sponsors/lucab85) ![](img/c519456ff1c2019c45035dbf8195f84a.png)