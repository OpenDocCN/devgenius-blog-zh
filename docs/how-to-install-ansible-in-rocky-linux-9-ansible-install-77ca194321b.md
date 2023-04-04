# 如何在 Rocky Linux 9 中安装 ansi ble—ansi ble install

> 原文：<https://blog.devgenius.io/how-to-install-ansible-in-rocky-linux-9-ansible-install-77ca194321b?source=collection_archive---------5----------------------->

## 如何使用 DNF 和“appstream”系统库在 Rocky Linux 9 内部安装和维护最新的 Ansible？

# 如何在 Rocky Linux 版本 9 中安装 Ansible？

今天我们将讨论使用 AppStream 系统库在 Rocky Linux 9 中安装和维护 Ansible 的更简单的方法。

我是卢卡·伯尔顿，欢迎来到今天的《神秘飞行员》节目。

# 如何在 Rocky Linux 9 中安装 Ansible

*   `ansible-core`包含在应用流存储库中
*   `ansible`套餐不可用

今天我们讨论的是如何在 Rocky Linux 9 中安装 Ansible。

在 Rocky Linux version 9 中安装和维护最新 Ansible 的更简单的方法是使用 AppStream 发行库中包含的`ansible-core`包。请注意，包`ansible`不再可用。没有必要使用额外的 EPEL 包存储库。

参见: [Ansible 术语— ansible 与 ansible-core 封装](https://www.ansiblepilot.com/articles/ansible-terminology-ansible-vs-ansible-core-packages/)。

# 链接

*   [https://rockylinux.org/news/rocky-linux-9-0-ga-release/](https://rockylinux.org/news/rocky-linux-9-0-ga-release/)

# 演示

让我们快速进入如何在 Rocky Linux 中安装 Ansible 最新版本的现场演示。我将使用 AppStream 发行库在 Rocky Linux 9 中安装`ansible-core`包。

## 密码

*   Install-Ansible-RockyLinux.sh

```
#!/bin/bash
sudo dnf install ansible-core
```

## 执行

```
ansible-pilot $ ssh [devops@rockylinux.example.com](mailto:devops@rockylinux.example.com)
[devops@rockylinux ~]$ sudo su
[root@rockylinux devops]# cat /etc/redhat-release 
Rocky Linux release 9.0 (Blue Onyx)
[root@rockylinux devops]# cat /etc/os-release 
NAME="Rocky Linux"
VERSION="9.0 (Blue Onyx)"
ID="rocky"
ID_LIKE="rhel centos fedora"
VERSION_ID="9.0"
PLATFORM_ID="platform:el9"
PRETTY_NAME="Rocky Linux 9.0 (Blue Onyx)"
ANSI_COLOR="0;32"
LOGO="fedora-logo-icon"
CPE_NAME="cpe:/o:rocky:rocky:9::baseos"
HOME_URL="[https://rockylinux.org/](https://rockylinux.org/)"
BUG_REPORT_URL="[https://bugs.rockylinux.org/](https://bugs.rockylinux.org/)"
ROCKY_SUPPORT_PRODUCT="Rocky-Linux-9"
ROCKY_SUPPORT_PRODUCT_VERSION="9.0"
REDHAT_SUPPORT_PRODUCT="Rocky Linux"
REDHAT_SUPPORT_PRODUCT_VERSION="9.0"
[root@rockylinux devops]# hostnamectl 
 Static hostname: rockylinux.example.com
       Icon name: computer-vm
         Chassis: vm 🖴
      Machine ID: 26419e43cdb741a2a2068070219bf8d9
         Boot ID: bed36d9812744b248bc8f674343c83c9
  Virtualization: oracle
Operating System: Rocky Linux 9.0 (Blue Onyx)      
     CPE OS Name: cpe:/o:rocky:rocky:9::baseos
          Kernel: Linux 5.14.0-70.13.1.el9_0.x86_64
    Architecture: x86-64
 Hardware Vendor: innotek GmbH
  Hardware Model: VirtualBox
[root@rockylinux devops]# uname -a
Linux rockylinux.example.com 5.14.0-70.13.1.el9_0.x86_64 #1 SMP PREEMPT Wed May 25 21:01:57 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
[root@rockylinux devops]# dnf search ansible
================================ Name & Summary Matched: ansible =================================
ansible-collection-microsoft-sql.noarch : The Ansible collection for Microsoft SQL Server
                                        : management
ansible-collection-redhat-rhel_mgmt.noarch : Ansible Collection of general system management and
                                           : utility modules and other plugins
ansible-freeipa-tests.noarch : ansible-freeipa tests
ansible-pcp.noarch : Ansible Metric collection for Performance Co-Pilot
ansible-test.x86_64 : Tool for testing ansible plugin and module code
====================================== Name Matched: ansible ======================================
ansible-core.x86_64 : SSH-based configuration management, deployment, and task execution system
ansible-freeipa.noarch : Roles and playbooks to deploy FreeIPA servers, replicas and clients
==================================== Summary Matched: ansible =====================================
rhc-worker-playbook.x86_64 : Python worker for Red Hat connector that launches Ansible Runner
[root@rockylinux devops]# dnf info ansible-core
Available Packages
Name         : ansible-core
Version      : 2.12.2
Release      : 1.el9.rocky.0.1
Architecture : x86_64
Size         : 2.0 M
Source       : ansible-core-2.12.2-1.el9.rocky.0.1.src.rpm
Repository   : appstream
Summary      : SSH-based configuration management, deployment, and task execution system
URL          : [http://ansible.com](http://ansible.com)
License      : GPLv3+
Description  : Ansible is a radically simple model-driven configuration management,
             : multi-node deployment, and remote task execution system. Ansible works
             : over SSH and does not require any software or daemons to be installed
             : on remote nodes. Extension modules can be written in any language and
             : are transferred to managed machines automatically.[root@rockylinux devops]# dnf list ansible-core
Available Packages
ansible-core.x86_64                        2.12.2-1.el9.rocky.0.1                         appstream
[root@rockylinux devops]# dnf install ansible-core
Last metadata expiration check: 1:08:45 ago on Tue 19 Jul 2022 10:15:38 AM UTC.
Dependencies resolved.
===================================================================================================
 Package                     Architecture  Version                          Repository        Size
===================================================================================================
Installing:
 ansible-core                x86_64        2.12.2-1.el9.rocky.0.1           appstream        2.0 M
Installing dependencies:
 emacs-filesystem            noarch        1:27.2-6.el9                     appstream        8.4 k
 git                         x86_64        2.31.1-2.el9.2                   appstream        120 k
 git-core                    x86_64        2.31.1-2.el9.2                   appstream        3.6 M
 git-core-doc                noarch        2.31.1-2.el9.2                   appstream        2.3 M
 perl-Error                  noarch        1:0.17029-7.el9                  appstream         41 k
 perl-Git                    noarch        2.31.1-2.el9.2                   appstream         43 k
 python3-babel               noarch        2.9.1-2.el9                      appstream        5.8 M
 python3-cffi                x86_64        1.14.5-5.el9                     appstream        241 k
 python3-cryptography        x86_64        36.0.1-1.el9_0                   appstream        1.1 M
 python3-jinja2              noarch        2.11.3-4.el9                     appstream        229 k
 python3-markupsafe          x86_64        1.1.1-12.el9                     appstream         32 k
 python3-packaging           noarch        20.9-5.el9                       appstream         69 k
 python3-ply                 noarch        3.11-14.el9                      appstream        103 k
 python3-pycparser           noarch        2.20-6.el9                       appstream        124 k
 python3-pytz                noarch        2021.1-4.el9                     appstream         48 k
 python3-pyyaml              x86_64        5.4.1-6.el9                      baseos           191 k
 python3-resolvelib          noarch        0.5.4-5.el9                      appstream         29 k
 sshpass                     x86_64        1.09-4.el9                       appstream         27 kTransaction Summary
===================================================================================================
Install  19 PackagesTotal download size: 16 M
Installed size: 77 M
Is this ok [y/N]: y
Downloading Packages:
(1/19): python3-pyyaml-5.4.1-6.el9.x86_64.rpm                      275 kB/s | 191 kB     00:00    
(2/19): python3-jinja2-2.11.3-4.el9.noarch.rpm                     328 kB/s | 229 kB     00:00    
(3/19): python3-packaging-20.9-5.el9.noarch.rpm                    554 kB/s |  69 kB     00:00    
(4/19): python3-resolvelib-0.5.4-5.el9.noarch.rpm                  304 kB/s |  29 kB     00:00    
(5/19): python3-pytz-2021.1-4.el9.noarch.rpm                       157 kB/s |  48 kB     00:00    
(6/19): python3-pycparser-2.20-6.el9.noarch.rpm                    724 kB/s | 124 kB     00:00    
(7/19): python3-ply-3.11-14.el9.noarch.rpm                         555 kB/s | 103 kB     00:00    
(8/19): python3-markupsafe-1.1.1-12.el9.x86_64.rpm                 321 kB/s |  32 kB     00:00    
(9/19): perl-Error-0.17029-7.el9.noarch.rpm                        372 kB/s |  41 kB     00:00    
(10/19): sshpass-1.09-4.el9.x86_64.rpm                             211 kB/s |  27 kB     00:00    
(11/19): python3-cffi-1.14.5-5.el9.x86_64.rpm                      692 kB/s | 241 kB     00:00    
(12/19): emacs-filesystem-27.2-6.el9.noarch.rpm                     57 kB/s | 8.4 kB     00:00    
(13/19): perl-Git-2.31.1-2.el9.2.noarch.rpm                        334 kB/s |  43 kB     00:00    
(14/19): python3-cryptography-36.0.1-1.el9_0.x86_64.rpm            916 kB/s | 1.1 MB     00:01    
(15/19): python3-babel-2.9.1-2.el9.noarch.rpm                      1.1 MB/s | 5.8 MB     00:05    
(16/19): git-core-doc-2.31.1-2.el9.2.noarch.rpm                    699 kB/s | 2.3 MB     00:03    
(17/19): git-2.31.1-2.el9.2.x86_64.rpm                             349 kB/s | 120 kB     00:00    
(18/19): git-core-2.31.1-2.el9.2.x86_64.rpm                        1.4 MB/s | 3.6 MB     00:02    
(19/19): ansible-core-2.12.2-1.el9.rocky.0.1.x86_64.rpm            1.7 MB/s | 2.0 MB     00:01    
---------------------------------------------------------------------------------------------------
Total                                                              2.2 MB/s |  16 MB     00:07     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                           1/1 
  Installing       : git-core-2.31.1-2.el9.2.x86_64                                           1/19 
  Installing       : git-core-doc-2.31.1-2.el9.2.noarch                                       2/19 
  Installing       : emacs-filesystem-1:27.2-6.el9.noarch                                     3/19 
  Installing       : sshpass-1.09-4.el9.x86_64                                                4/19 
  Installing       : perl-Error-1:0.17029-7.el9.noarch                                        5/19 
  Installing       : git-2.31.1-2.el9.2.x86_64                                                6/19 
  Installing       : perl-Git-2.31.1-2.el9.2.noarch                                           7/19 
  Installing       : python3-markupsafe-1.1.1-12.el9.x86_64                                   8/19 
  Installing       : python3-ply-3.11-14.el9.noarch                                           9/19 
  Installing       : python3-pycparser-2.20-6.el9.noarch                                     10/19 
  Installing       : python3-cffi-1.14.5-5.el9.x86_64                                        11/19 
  Installing       : python3-cryptography-36.0.1-1.el9_0.x86_64                              12/19 
  Installing       : python3-resolvelib-0.5.4-5.el9.noarch                                   13/19 
  Installing       : python3-packaging-20.9-5.el9.noarch                                     14/19 
  Installing       : python3-pytz-2021.1-4.el9.noarch                                        15/19 
  Installing       : python3-babel-2.9.1-2.el9.noarch                                        16/19 
  Installing       : python3-jinja2-2.11.3-4.el9.noarch                                      17/19 
  Installing       : python3-pyyaml-5.4.1-6.el9.x86_64                                       18/19 
  Installing       : ansible-core-2.12.2-1.el9.rocky.0.1.x86_64                              19/19 
  Running scriptlet: ansible-core-2.12.2-1.el9.rocky.0.1.x86_64                              19/19 
  Verifying        : python3-pyyaml-5.4.1-6.el9.x86_64                                        1/19 
  Verifying        : python3-babel-2.9.1-2.el9.noarch                                         2/19 
  Verifying        : python3-jinja2-2.11.3-4.el9.noarch                                       3/19 
  Verifying        : python3-pytz-2021.1-4.el9.noarch                                         4/19 
  Verifying        : python3-packaging-20.9-5.el9.noarch                                      5/19 
  Verifying        : python3-resolvelib-0.5.4-5.el9.noarch                                    6/19 
  Verifying        : python3-pycparser-2.20-6.el9.noarch                                      7/19 
  Verifying        : python3-ply-3.11-14.el9.noarch                                           8/19 
  Verifying        : python3-markupsafe-1.1.1-12.el9.x86_64                                   9/19 
  Verifying        : perl-Error-1:0.17029-7.el9.noarch                                       10/19 
  Verifying        : python3-cffi-1.14.5-5.el9.x86_64                                        11/19 
  Verifying        : sshpass-1.09-4.el9.x86_64                                               12/19 
  Verifying        : emacs-filesystem-1:27.2-6.el9.noarch                                    13/19 
  Verifying        : python3-cryptography-36.0.1-1.el9_0.x86_64                              14/19 
  Verifying        : perl-Git-2.31.1-2.el9.2.noarch                                          15/19 
  Verifying        : git-core-doc-2.31.1-2.el9.2.noarch                                      16/19 
  Verifying        : git-core-2.31.1-2.el9.2.x86_64                                          17/19 
  Verifying        : git-2.31.1-2.el9.2.x86_64                                               18/19 
  Verifying        : ansible-core-2.12.2-1.el9.rocky.0.1.x86_64                              19/19Installed:
  ansible-core-2.12.2-1.el9.rocky.0.1.x86_64       emacs-filesystem-1:27.2-6.el9.noarch            
  git-2.31.1-2.el9.2.x86_64                        git-core-2.31.1-2.el9.2.x86_64                  
  git-core-doc-2.31.1-2.el9.2.noarch               perl-Error-1:0.17029-7.el9.noarch               
  perl-Git-2.31.1-2.el9.2.noarch                   python3-babel-2.9.1-2.el9.noarch                
  python3-cffi-1.14.5-5.el9.x86_64                 python3-cryptography-36.0.1-1.el9_0.x86_64      
  python3-jinja2-2.11.3-4.el9.noarch               python3-markupsafe-1.1.1-12.el9.x86_64          
  python3-packaging-20.9-5.el9.noarch              python3-ply-3.11-14.el9.noarch                  
  python3-pycparser-2.20-6.el9.noarch              python3-pytz-2021.1-4.el9.noarch                
  python3-pyyaml-5.4.1-6.el9.x86_64                python3-resolvelib-0.5.4-5.el9.noarch           
  sshpass-1.09-4.el9.x86_64Complete!
[root@rockylinux devops]# ansible --version
ansible [core 2.12.2]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3.9/site-packages/ansible
  ansible collection location = /root/.ansible/collections:/usr/share/ansible/collections
  executable location = /bin/ansible
  python version = 3.9.10 (main, Feb  9 2022, 00:00:00) [GCC 11.2.1 20220127 (Red Hat 11.2.1-9)]
  jinja version = 2.11.3
  libyaml = True
[root@rockylinux devops]# dnf list ansible-core
Installed Packages
ansible-core.x86_64                        2.12.2-1.el9.rocky.0.1                        [@appstream](http://twitter.com/appstream)
[root@rockylinux devops]# dnf info ansible-core
Installed Packages
Name         : ansible-core
Version      : 2.12.2
Release      : 1.el9.rocky.0.1
Architecture : x86_64
Size         : 9.3 M
Source       : ansible-core-2.12.2-1.el9.rocky.0.1.src.rpm
Repository   : [@System](http://twitter.com/System)
From repo    : appstream
Summary      : SSH-based configuration management, deployment, and task execution system
URL          : [http://ansible.com](http://ansible.com)
License      : GPLv3+
Description  : Ansible is a radically simple model-driven configuration management,
             : multi-node deployment, and remote task execution system. Ansible works
             : over SSH and does not require any software or daemons to be installed
             : on remote nodes. Extension modules can be written in any language and
             : are transferred to managed machines automatically.[root@rockylinux devops]#
```

## 执行前

```
[root@rockylinux devops]# dnf list ansible-core
Last metadata expiration check: 1:08:31 ago on Tue 19 Jul 2022 10:15:38 AM UTC.
Available Packages
ansible-core.x86_64                        2.12.2-1.el9.rocky.0.1                         appstream
[root@rockylinux devops]# dnf info ansible-core
Last metadata expiration check: 1:08:24 ago on Tue 19 Jul 2022 10:15:38 AM UTC.
Available Packages
Name         : ansible-core
Version      : 2.12.2
Release      : 1.el9.rocky.0.1
Architecture : x86_64
Size         : 2.0 M
Source       : ansible-core-2.12.2-1.el9.rocky.0.1.src.rpm
Repository   : appstream
Summary      : SSH-based configuration management, deployment, and task execution system
URL          : [http://ansible.com](http://ansible.com)
License      : GPLv3+
Description  : Ansible is a radically simple model-driven configuration management,
             : multi-node deployment, and remote task execution system. Ansible works
             : over SSH and does not require any software or daemons to be installed
             : on remote nodes. Extension modules can be written in any language and
             : are transferred to managed machines automatically.[root@rockylinux devops]#
```

## 执行后

```
[root@rockylinux devops]# dnf list ansible-core
Last metadata expiration check: 1:09:53 ago on Tue 19 Jul 2022 10:15:38 AM UTC.
Installed Packages
ansible-core.x86_64                        2.12.2-1.el9.rocky.0.1                        [@appstream](http://twitter.com/appstream)
[root@rockylinux devops]# dnf info ansible-core
Last metadata expiration check: 1:10:05 ago on Tue 19 Jul 2022 10:15:38 AM UTC.
Installed Packages
Name         : ansible-core
Version      : 2.12.2
Release      : 1.el9.rocky.0.1
Architecture : x86_64
Size         : 9.3 M
Source       : ansible-core-2.12.2-1.el9.rocky.0.1.src.rpm
Repository   : [@System](http://twitter.com/System)
From repo    : appstream
Summary      : SSH-based configuration management, deployment, and task execution system
URL          : [http://ansible.com](http://ansible.com)
License      : GPLv3+
Description  : Ansible is a radically simple model-driven configuration management,
             : multi-node deployment, and remote task execution system. Ansible works
             : over SSH and does not require any software or daemons to be installed
             : on remote nodes. Extension modules can be written in any language and
             : are transferred to managed machines automatically.[root@rockylinux devops]#
```

[](https://github.com/lucab85/ansible-pilot) [## GitHub-lucab 85/ansi ble-Pilot:ansi ble Pilot YouTube 频道代码库

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/lucab85/ansible-pilot) 

# 概述

现在你知道如何使用 AppStream 系统库在 Rocky Linux 中安装 Ansible 的最新版本了。

我希望你喜欢读这篇文章。如果你愿意支持我成为一名作家，考虑注册[成为一名媒体成员](https://ansiblepilot.medium.com/membership)。每月只需 5 美元，你就可以无限制地使用 Medium。

订阅 [YouTube 频道](https://www.youtube.com/channel/UC5MNbTYRHSCu9vAki3z9SmA)、 [Medium](https://ansiblepilot.medium.com/) 和[网站](https://www.ansiblepilot.com/)，不要错过 Ansible 试播的下一集。

# Ansible 的最佳资源

# 视频课程

*   [通过 200 多个示例学习 Ansible Automation&实践课程:通过一些真实的例子学习 Ansible，了解如何使用最常见的模块和 ansi ble 行动手册](https://www.udemy.com/course/ansible-by-examples-devops/?referralCode=8E065F6D6F8622A3DEC8)

# 书

*   [举例说明:针对 Linux 和 Windows 系统管理员和开发人员的 200 多个自动化示例](https://leanpub.com/ansiblebyexamples)
*   [负责 Windows 示例:30 多个针对 Windows 系统管理员和开发人员的自动化示例](https://www.amazon.com/dp/B09VCTP1L4)
*   [ansi ble For Linux by Examples:100 多个针对 Linux 系统管理员和开发人员的自动化示例](https://www.amazon.com/dp/B09VCX4CVD)
*   [Ansible Linux 文件系统示例:30 多个针对现代 IT 基础设施的 Linux 文件和目录操作自动化示例](https://www.amazon.com/dp/B09X4TF8P8)
*   [通过示例负责容器和 Kubernetes:10 多个自动化示例来自动化容器、Kubernetes 和 OpenShift](https://www.amazon.com/dp/B09XVDWD4W)
*   [负责安全示例:100 多个自动化示例，用于自动化现代 IT 基础架构的安全和验证合规性](https://www.amazon.com/dp/B09VHFLSPP)
*   [可行的技巧和窍门:10 多个可行的例子来节省时间和自动化更多的任务](https://www.amazon.com/dp/B09ZJ4GYM2)
*   [Ansible Linux 用户&按示例分组:20 多个关于现代 IT 基础设施的 Linux 用户和分组操作的自动化示例](https://www.amazon.com/dp/B09X6BYY8W)
*   [ansi ble For VMware by Examples:10 多个自动化您的 VMware 基础架构的示例(Ansible by Examples)](https://www.amazon.com/dp/B0B18QG44S)
*   [Ansible For PostgreSQL by Examples:10 多个自动化 PostgreSQL 数据库的示例](https://www.amazon.com/dp/B0B3RZZQVG)

# 捐赠

[](https://patreon.com/lucaberton) [## 卢卡·伯尔顿正在为 Ansible | Patreon 创建软件开源

### 今天就成为卢卡·伯尔顿的赞助人:获得世界上最大会员的独家内容和体验…

patreon.com](https://patreon.com/lucaberton) [](https://github.com/sponsors/lucab85) [## GitHub 赞助商上的赞助商@lucab85

### 我是一个活跃的开源贡献者，参与到了 Ansible 社区中，尽管我到处都是。@lucab85 的…

github.com](https://github.com/sponsors/lucab85) ![](img/1bde2e66fa319162bf90f711f30a8a71.png)