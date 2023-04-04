# 在可转换的文件模块行文件中搜索字符串

> 原文：<https://blog.devgenius.io/search-for-a-string-in-a-file-ansible-module-lineinfile-fcab6e3444c0?source=collection_archive---------1----------------------->

## 如何使用 Ansible Playbook 和 lineinfile 模块在“/etc/ssh/sshd_config”文件中自动搜索字符串“PasswordAuthentication no”。

# 如何用 Ansible 在文件中搜索字符串？

我将向您展示一些简单的可解析代码。

我是卢卡·伯尔顿，欢迎来到今天的《神秘飞行员》节目。

# 可变模线文件

*   `ansible.builtin.lineinfile`
*   插入/更新/删除文件中的单行文本

今天我们讨论的是 Ansible 模块“`lineinfile`”。

全名是``ansible.builtin.lineinfile``,这意味着它是 ansible“内置”和附带的模块集合的一部分。

这是一个相当稳定的模块，已经推出多年，它支持多种操作系统。

您可以在文件中插入、更新和删除单行文本。

# 因素

*   **路径** *字符串—* 文件路径
*   行*字符串—* 文本
*   insertafter/insertbefore *字符串—***of**/正则表达式
*   验证*字符串—* 验证命令
*   创建*布尔值—* 不存在时创建
*   状态*字符串—* **出席**/缺席
*   模式/所有者/组-权限
*   setype/seuser/selevel — SELinux

这个模块有一些参数来执行任何任务。

唯一需要的是“path”，您可以在这里指定要编辑的文件的文件系统路径。

“行”是我们想要插入到文件中的文本行，简单！

默认情况下，文本将被插入到文件的末尾，但是我们可以使用 insertafter/insertbefore 在特定位置对其进行个性化设置。

如果有任何工具来验证文件，我们可以在 validate 参数中指定，这对配置文件非常有用。

如果文件不存在，我们也可以“创建”它！

通常，我们希望插入一个文本行，但是我们也可以在缺少参数的情况下删除使用状态。

让我强调一下，我们还可以指定一些权限或 SELinux 属性。

# 链接

*   [https://docs . ansi ble . com/ansi ble/latest/collections/ansi ble/builtin/line infile _ module . html](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/lineinfile_module.html)

# 演示

如何在文件中搜索字符串？

如何仅使用 Ansible 内置`lineinfile`模块在文件中搜索模式并返回结果。

## 密码

```
---
- name: search demo
  hosts: all
  vars:
    myfile: "/etc/ssh/sshd_config"
    myline: 'PasswordAuthentication no'
  become: true
  tasks:
    - name: string found
      ansible.builtin.lineinfile:
        name: "{{ myfile }}"
        line: "{{ myline }}"
        state: present
      check_mode: true
      register: conf
      failed_when: (conf is changed) or (conf is failed)
```

## 字符串存在

*   远程主机

```
$ ssh [devops@demo.example.com](mailto:devops@demo.example.com)
[devops@demo ~]$ sudo su
[root@demo devops]# grep 'PasswordAuthentication no' /etc/ssh/sshd_config 
PasswordAuthentication no
[root@demo devops]#
```

*   可执行

```
 $ ansible-playbook -i virtualmachines/demo/inventory file_management/file_search.ymlPLAY [search demo] ********************************************************************************TASK [Gathering Facts] ****************************************************************************
ok: [demo.example.com]TASK [string found] *******************************************************************************
ok: [demo.example.com]PLAY RECAP ****************************************************************************************
demo.example.com           : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## 字符串不同

*   远程主机

```
$ ssh [devops@demo.example.com](mailto:devops@demo.example.com)
[devops@demo ~]$ sudo su
[root@demo devops]# vim /etc/ssh/sshd_config 
[root@demo devops]# grep 'PasswordAuthentication' /etc/ssh/sshd_config 
PasswordAuthentication yes
[root@demo devops]#
```

*   可执行

```
$ ansible-playbook -i virtualmachines/demo/inventory file_management/file_search.ymlPLAY [search demo] ********************************************************************************TASK [Gathering Facts] ****************************************************************************
ok: [demo.example.com]TASK [string found] *******************************************************************************
fatal: [demo.example.com]: FAILED! => {"backup": "", "changed": true, "failed_when_result": true, "msg": "line added"}PLAY RECAP ****************************************************************************************
demo.example.com           : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
```

## 文件不存在

*   远程主机

```
$ ssh [devops@demo.example.com](mailto:devops@demo.example.com)
[devops@demo ~]$ sudo su
[root@demo ssh]# ls -al /etc/ssh/sshd_config
ls: cannot access '/etc/ssh/sshd_config': No such file or directory
[root@demo ssh]#
```

*   可执行

```
$ ansible-playbook -i virtualmachines/demo/inventory file_management/file_search.yml
PLAY [search demo] ********************************************************************************
TASK [Gathering Facts] ****************************************************************************
ok: [demo.example.com]
TASK [string found] *******************************************************************************
fatal: [demo.example.com]: FAILED! => {"changed": false, "failed_when_result": true, "msg": "Destination /etc/ssh/sshd_config does not exist !", "rc": 257}
PLAY RECAP ****************************************************************************************
demo.example.com           : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
```

[](https://github.com/lucab85/ansible-pilot) [## GitHub-lucab 85/ansi ble-Pilot:ansi ble Pilot YouTube 频道代码库

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/lucab85/ansible-pilot) 

# 概述

现在，您知道了如何使用 Ansible 在文件中搜索字符串，以及如何在行动手册中成功使用。

我希望你喜欢读这篇文章。如果你愿意支持我成为一名作家，考虑注册[成为一名媒体成员](https://ansiblepilot.medium.com/membership)。每月只需 5 美元，你就可以无限制地使用 Medium。

订阅 [YouTube 频道](https://www.youtube.com/channel/UC5MNbTYRHSCu9vAki3z9SmA)、 [Medium](https://ansiblepilot.medium.com/) 和[网站](https://www.ansiblepilot.com/)，不要错过 Ansible 试播的下一集。

# Ansible 的最佳资源

# 视频课程

*   [通过 200 多个示例学习 Ansible Automation&实践课程:通过一些真实的例子学习 Ansible，了解如何使用最常见的模块和 ansi ble 行动手册](https://www.udemy.com/course/ansible-by-examples-devops/?referralCode=8E065F6D6F8622A3DEC8)

# 书

*   [可通过示例回答:针对 Linux 和 Windows 系统管理员和开发人员的 200 多个自动化示例](https://leanpub.com/ansiblebyexamples)
*   [适用于 Windows 的示例:针对 Windows 系统管理员和开发人员的 30 多个自动化示例](https://www.amazon.com/dp/B09VCTP1L4)
*   [Ansible For Linux by Examples:针对 Linux 系统管理员和开发人员的 100 多个自动化示例](https://www.amazon.com/dp/B09VCX4CVD)
*   [Ansible Linux 文件系统示例:30 多个针对现代 IT 基础设施的 Linux 文件和目录操作自动化示例](https://www.amazon.com/dp/B09X4TF8P8)
*   [通过示例负责容器和 Kubernetes:10 多个自动化示例来自动化容器、Kubernetes 和 OpenShift](https://www.amazon.com/dp/B09XVDWD4W)
*   [负责安全示例:100 多个自动化示例，用于自动化安全和验证 IT 现代化基础设施的合规性](https://www.amazon.com/dp/B09VHFLSPP)
*   [可行的技巧和窍门:10 多个可行的例子来节省时间和自动化更多的任务](https://www.amazon.com/dp/B09ZJ4GYM2)
*   [Ansible Linux 用户&按示例分组:20 多个关于现代 IT 基础设施的 Linux 用户和分组操作的自动化示例](https://www.amazon.com/dp/B09X6BYY8W)
*   [ansi ble For VMware by Examples:10 多个自动化您的 VMware 基础架构的示例(Ansible by Examples)](https://www.amazon.com/dp/B0B18QG44S)
*   [Ansible For PostgreSQL by Examples:10+个自动化 PostgreSQL 数据库的示例](https://www.amazon.com/dp/B0B3RZZQVG)

# 捐赠

[](https://patreon.com/lucaberton) [## 卢卡·伯尔顿正在为 Ansible | Patreon 创建软件开源

### 今天就成为卢卡·伯尔顿的赞助人:获得世界上最大会员的独家内容和体验…

patreon.com](https://patreon.com/lucaberton) [](https://github.com/sponsors/lucab85) [## GitHub 赞助商上的赞助商@lucab85

### 我是一个活跃的开源贡献者，参与到了 Ansible 社区中，尽管我到处都是。@lucab85 的…

github.com](https://github.com/sponsors/lucab85) ![](img/1642ec82bcf9390ab76da0ec1a014d08.png)