# 如何在 Fedora 37 中安装 Ansible

> 原文：<https://blog.devgenius.io/how-to-install-ansible-in-fedora-37-a3b4709c33c3?source=collection_archive---------4----------------------->

如何使用系统库在 Fedora 37 内部安装和维护最新版本的 Ansible，并附有实用演示。

如何在 Fedora 版本 37 中安装 Ansible？

今天我们将讨论使用系统库在 Fedora 37 中安装和维护 Ansible 的更简单的方法。

我是卢卡·伯尔顿，欢迎来到今天的《神秘飞行员》节目。

# 如何在 Fedora 37 中安装 Ansible

今天我们讨论的是如何在 Fedora 37 中安装 Ansible。

好消息是 Ansible 包含在默认的存储库中，所以您可以用您常用的包管理器简单地安装它。

您可以在更新的存储库中找到 Ansible 的最新版本。

目前，`ansible-core`的最新版本为 2.14，`ansible`的最新版本为 6.4。

# 演示

让我们快速进入如何在 Fedora 中安装 Ansible 最新版本的现场演示。

## 密码

*   install-Ansible-Fedora.sh

```
#!/bin/bash
sudo dnf update
sudo dnf install ansible
```

## 执行

```
[luca@fedora ~]$ sudo su
[sudo] password for luca: 
[root@fedora luca]# cat /etc/redhat-release 
Fedora release 37 (Thirty Seven)
[root@fedora luca]# hostnamectl
   Static hostname: n/a                                  
Transient hostname: fedora
         Icon name: computer-vm
           Chassis: vm 🖴
        Machine ID: d2529b15dd1248ff98eb849bea4769ff
           Boot ID: 25ddcebe009247d285a8acef12f3d2ca
    Virtualization: vmware
  Operating System: Fedora Linux 37 (Workstation Edition)
       CPE OS Name: cpe:/o:fedoraproject:fedora:37
            Kernel: Linux 6.0.7-301.fc37.aarch64
      Architecture: arm64
   Hardware Vendor: VMware, Inc.
    Hardware Model: VMware20,1
  Firmware Version: VMW201.00V.20432347.BA64.2209100127
[root@fedora luca]# cat /etc/os-release 
NAME="Fedora Linux"
VERSION="37 (Workstation Edition)"
ID=fedora
VERSION_ID=37
VERSION_CODENAME=""
PLATFORM_ID="platform:f37"
PRETTY_NAME="Fedora Linux 37 (Workstation Edition)"
ANSI_COLOR="0;38;2;60;110;180"
LOGO=fedora-logo-icon
CPE_NAME="cpe:/o:fedoraproject:fedora:37"
DEFAULT_HOSTNAME="fedora"
HOME_URL="https://fedoraproject.org/"
DOCUMENTATION_URL="https://docs.fedoraproject.org/en-US/fedora/f37/system-administrators-guide/"
SUPPORT_URL="https://ask.fedoraproject.org/"
BUG_REPORT_URL="https://bugzilla.redhat.com/"
REDHAT_BUGZILLA_PRODUCT="Fedora"
REDHAT_BUGZILLA_PRODUCT_VERSION=37
REDHAT_SUPPORT_PRODUCT="Fedora"
REDHAT_SUPPORT_PRODUCT_VERSION=37
VARIANT="Workstation Edition"
VARIANT_ID=workstation
[root@fedora luca]# ansible
bash: ansible: command not found...
Install package 'ansible-core' to provide command 'ansible'? [N/y] n

[root@fedora luca]# dnf list available ansible
Copr repo for PyCharm owned  375  B/s | 341  B     00:00    
Last metadata expiration check: 1:34:46 ago on Tue 29 Nov 2022 06:32:17 PM GMT.
Available Packages
ansible.noarch            7.0.0~b1-1.fc37             updates
[root@fedora luca]# dnf list available ansible-core
Last metadata expiration check: 1:35:06 ago on Tue 29 Nov 2022 06:32:17 PM GMT.
Available Packages
ansible-core.noarch           2.14.0-1.fc37           updates
[root@fedora luca]# rpm -qa | grep ansible
[root@fedora luca]# ansible --version
bash: ansible: command not found...
Install package 'ansible-core' to provide command 'ansible'? [N/y] n

[root@fedora luca]# dnf install ansible
Last metadata expiration check: 1:35:51 ago on Tue 29 Nov 2022 06:32:17 PM GMT.
Dependencies resolved.
=============================================================
 Package             Arch    Version          Repo      Size
=============================================================
Installing:
 ansible             noarch  7.0.0~b1-1.fc37  updates   43 M
Installing dependencies:
 ansible-core        noarch  2.14.0-1.fc37    updates  3.6 M
 python3-jinja2      noarch  3.0.3-5.fc37     fedora   630 k
 python3-resolvelib  noarch  0.5.5-6.fc37     fedora    43 k

Transaction Summary
=============================================================
Install  4 Packages

Total download size: 48 M
Installed size: 334 M
Is this ok [y/N]: y
Downloading Packages:
(1/4): python3-resolvelib-0\.  92 kB/s |  43 kB     00:00    
(2/4): python3-jinja2-3.0.3- 557 kB/s | 630 kB     00:01    
(3/4): ansible-core-2.14.0-1 943 kB/s | 3.6 MB     00:03    
(4/4): ansible-7.0.0~b1-1.fc 1.5 MB/s |  43 MB     00:29    
-------------------------------------------------------------
Total                        1.5 MB/s |  48 MB     00:31     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                     1/1 
  Installing       : python3-resolvelib-0.5.5-6.fc37.n   1/4 
  Installing       : python3-jinja2-3.0.3-5.fc37.noarc   2/4 
  Installing       : ansible-core-2.14.0-1.fc37.noarch   3/4 
  Installing       : ansible-7.0.0~b1-1.fc37.noarch      4/4 
  Running scriptlet: ansible-7.0.0~b1-1.fc37.noarch      4/4 
  Verifying        : python3-jinja2-3.0.3-5.fc37.noarc   1/4 
  Verifying        : python3-resolvelib-0.5.5-6.fc37.n   2/4 
  Verifying        : ansible-7.0.0~b1-1.fc37.noarch      3/4 
  Verifying        : ansible-core-2.14.0-1.fc37.noarch   4/4 

Installed:
  ansible-7.0.0~b1-1.fc37.noarch                             
  ansible-core-2.14.0-1.fc37.noarch                          
  python3-jinja2-3.0.3-5.fc37.noarch                         
  python3-resolvelib-0.5.5-6.fc37.noarch                     

Complete!
[root@fedora luca]# ansible --version
ansible [core 2.14.0]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3.11/site-packages/ansible
  ansible collection location = /root/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.11.0 (main, Oct 24 2022, 00:00:00) [GCC 12.2.1 20220819 (Red Hat 12.2.1-2)] (/usr/bin/python3)
  jinja version = 3.0.3
  libyaml = True
```

## 执行前

```
[root@fedora luca]# ansible
bash: ansible: command not found...
Install package 'ansible-core' to provide command 'ansible'? [N/y] n

[root@fedora luca]# dnf list available ansible
Copr repo for PyCharm owned  375  B/s | 341  B     00:00    
Last metadata expiration check: 1:34:46 ago on Tue 29 Nov 2022 06:32:17 PM GMT.
Available Packages
ansible.noarch            7.0.0~b1-1.fc37             updates
[root@fedora luca]# dnf list available ansible-core
Last metadata expiration check: 1:35:06 ago on Tue 29 Nov 2022 06:32:17 PM GMT.
Available Packages
ansible-core.noarch           2.14.0-1.fc37           updates
[root@fedora luca]# rpm -qa | grep ansible
[root@fedora luca]# ansible --version
bash: ansible: command not found...
Install package 'ansible-core' to provide command 'ansible'? [N/y] 
```

## 执行后

```
[root@fedora luca]# rpm -qa | grep ansible
ansible-core-2.14.0-1.fc37.noarch
ansible-7.0.0~b1-1.fc37.noarch
[root@fedora luca]# dnf list installed ansible
Installed Packages
ansible.noarch            7.0.0~b1-1.fc37            @updates
[root@fedora luca]# dnf list installed ansible-core
Installed Packages
ansible-core.noarch          2.14.0-1.fc37           @updates
[root@fedora luca]# ansible --version
ansible [core 2.14.0]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3.11/site-packages/ansible
  ansible collection location = /root/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.11.0 (main, Oct 24 2022, 00:00:00) [GCC 12.2.1 20220819 (Red Hat 12.2.1-2)] (/usr/bin/python3)
  jinja version = 3.0.3
  libyaml = True
[root@fedora luca]#
```

# 概述

现在你知道了如何使用系统库和 DNF 包管理器在 Fedora 中安装 Ansible 的最新版本。订阅 [YouTube 频道](https://www.youtube.com/c/ansiblepilot)、 [Medium](https://ansiblepilot.medium.com/) 、[网站](https://www.ansiblepilot.com/)、 [Twitter](https://twitter.com/ansiblepilot) 和 [Substack](https://ansiblepilot.substack.com/) 以免错过下一集 Ansible 试播集。

# Ansible 的最佳资源

## 视频课程

*   [通过 200 多个示例学习 Ansible Automation&实践课程:通过一些真实的例子学习 Ansible，了解如何使用最常见的模块和 ansi ble 行动手册](https://click.linksynergy.com/deeplink?id=euGmLrdj*Ec&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fansible-by-examples-devops%2F%3FreferralCode%3D8E065F6D6F8622A3DEC8)

## 书

*   [Ansible For VMware by Examples:自动化您的 VMware 基础架构的逐步指南](https://www.amazon.com/Ansible-VMware-Examples-Step-Step/dp/1484288785/)
*   [可通过示例回答:针对 Linux 和 Windows 系统管理员和开发人员的 200 多个自动化示例](https://leanpub.com/ansiblebyexamples)
*   [适用于 Windows 的示例:50 多个针对 Windows 系统管理员和开发人员的自动化示例](https://leanpub.com/ansibleforwindowsbyexamples)
*   [ansi ble For Linux by Examples:100 多个针对 Linux 系统管理员和开发人员的自动化示例](https://leanpub.com/ansibleforlinuxbyexamples)
*   [Ansible Linux 文件系统示例:40 多个针对现代 IT 基础设施的 Linux 文件和目录操作自动化示例](https://leanpub.com/linuxfileanddirectorybyansibleexamples)
*   [通过示例负责容器和 Kubernetes:20 多个自动化示例来自动化容器、Kubernetes 和 OpenShift](https://leanpub.com/ansible-for-kubernetes-by-examples)
*   [负责安全示例:100 多个自动化示例，用于自动化现代 IT 基础设施的安全和验证合规性](https://leanpub.com/ansibleforsecuritybyexamples)
*   [可行的技巧和窍门:10 多个可行的例子来节省时间和自动化更多的任务](https://leanpub.com/ansible-tips-and-tricks)
*   [Ansible Linux 用户&按示例分组:20 多个关于现代 IT 基础设施的 Linux 用户和分组操作的自动化示例](https://leanpub.com/ansiblelinuxusersandgroupsbyexamples)
*   [Ansible For PostgreSQL by Examples:10 多个自动化 PostgreSQL 数据库的示例](https://leanpub.com/ansible-for-postgresql-by-examples)
*   [ansi ble For Amazon Web Services AWS By Examples:10 多个自动化 AWS 现代基础设施的示例](https://leanpub.com/ansible-for-aws-by-examples)

# 捐赠

[](https://patreon.com/lucaberton) [## 卢卡·伯尔顿正在为 Ansible | Patreon 创建软件开源

### 今天就成为卢卡·伯尔顿的赞助人:获得世界上最大会员的独家内容和体验…

patreon.com](https://patreon.com/lucaberton) [](https://github.com/sponsors/lucab85) [## GitHub 赞助商上的赞助商@lucab85

### 我是一个活跃的开源贡献者，参与到了 Ansible 社区中，尽管我到处都是。1 个赞助商有…

github.com](https://github.com/sponsors/lucab85) ![](img/83db433281deddb929d33a4104d5ef44.png)