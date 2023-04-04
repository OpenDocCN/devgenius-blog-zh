# 从文件和文件夹创建 ISO 映像— Ansible 模块 iso_create

> 原文：<https://blog.devgenius.io/create-an-iso-image-from-files-and-folders-ansible-module-iso-create-35487046051e?source=collection_archive---------12----------------------->

## 如何使用 Ansible Playbook 和 iso_create 模块自动创建包含“helloworld.txt”文件的“test . iso”ISO9660 文件。

如何用 Ansible 从文件和文件夹创建 ISO 镜像？

我将向您展示一个带有一些简单的 Ansible 代码的现场演示。

我是卢卡·伯尔顿，欢迎来到今天的《神秘飞行员》

# 从文件和文件夹创建 ISO 映像

*   `community.general.iso_create`
*   用指定的文件或文件夹生成 ISO 文件

我们来说说 Ansible 模块``iso_create``。

全称是“community.general.iso_create ”,这意味着它是 Ansible 社区维护的模块“`community.general`”集合的一部分。

这个模块需要额外的``pycdlib` ` Python 库。您可以使用 Python 包安装程序 PIP 轻松安装``pycdlib` ` Python 库。

该模块的目的是用指定的文件或文件夹生成 ISO 文件。

# 因素

*   `**dest_iso**` *路径—* ISO 文件绝对路径
*   `**src_files**` *列表—* 源文件或文件夹的绝对路径
*   `interchange_level` *整数—* ISO9660 标准(1–4)
*   `joliet` *整数—* Joliet 扩展(1–3)
*   `rock_ridge` *弦—* 岩脊延伸(1.09，1.10，1.12)
*   `udf` *布尔—* 否/是 UDF 支持 2.60

让我总结一下模块“iso_create”的主要参数。

必需的参数是“`dest_iso`”和“`src_files`”。

“`dest_iso`”参数包含根据 ISO9660 标准生成的 ISO 文件绝对路径。

“src_files”参数包含源文件或文件夹的绝对路径列表。

您可能希望将“interchange_level”参数设置为 4，因为它允许您指定支持第四个 ISO9660 标准中的哪一个，默认为“1”。

1 级是最严格的标准，4 级是最宽松的标准。

对于从 1 到 3 的所有 ISO9660 级别，所有文件名都被限制为大写字母、数字和下划线(_)。文件名限制为 31 个字符，目录嵌套限制为 8 级，路径名限制为 255 个字符。

级别 1 允许名称最多包含 8 个字符，扩展名最多包含 3 个字符。

Joliet 是一个流行的 ISO9660 扩展，在类似 Windows 的环境中，您可以将“`joliet`”参数设置为三，以支持长达 103 个字符的文件名(根据`mkisofs`文档)。

Rock Ridge 是类 Unix 操作系统中另一个流行的 ISO9660 扩展，支持长达 255 字节的文件名和软/硬链接。您可以根据“`rock_ridge`”参数启用“`1.12`”规格设置。

另一个流行的扩展是通用磁盘格式(UDF ),它允许像通用文件系统一样在磁盘上创建、删除和更改文件。您可以通过设置“`udf`”布尔值来启用 UDF 规范`2.60`。

# 链接

*   [https://docs . ansi ble . com/ansi ble/latest/collections/community/general/iso _ create _ module . html](https://docs.ansible.com/ansible/latest/collections/community/general/iso_create_module.html)

# 演示

让我们跳到现实生活中可行的剧本，从文件和文件夹中创建 ISO 映像。

我将向您展示如何在用户的`devops`主目录中使用文件“helloworld.txt”创建一个“example.iso”映像。

## 密码

*   create_iso.yml

```
---
- name: iso_create module demo
  hosts: all
  tasks:
    - name: Create an ISO file
      community.general.iso_create:
        src_files:
          - /home/devops/helloworld.txt
        dest_iso: /home/devops/test.iso
        interchange_level: 4
        joliet: 3
```

## 执行

```
ansible-pilot $ ansible-playbook -i virtualmachines/demo/inventory file_management/create_iso.ymlPLAY [iso_create module demo] *********************************************************************TASK [Gathering Facts] ****************************************************************************
ok: [demo.example.com]TASK [Create an ISO file] *************************************************************************
changed: [demo.example.com]PLAY RECAP ****************************************************************************************
demo.example.com           : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0ansible-pilot $
```

## 非幂等的

```
ansible-pilot $ ansible-playbook -i virtualmachines/demo/inventory file_management/create_iso.ymlPLAY [iso_create module demo] *********************************************************************TASK [Gathering Facts] ****************************************************************************
ok: [demo.example.com]TASK [Create an ISO file] *************************************************************************
changed: [demo.example.com]PLAY RECAP ****************************************************************************************
demo.example.com           : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0ansible-pilot $
```

## 执行前

```
$ ssh [devops@demo.example.com](mailto:devops@demo.example.com)
[devops@demo ~]$ ls
helloworld.txt
[devops@demo ~]$
```

## 执行后

```
ansible-pilot $ ssh [devops@demo.example.com](mailto:devops@demo.example.com)                                                      
[devops@demo ~]$ ls
helloworld.txt  test.iso
[devops@demo ~]$ file test.iso 
test.iso: ISO 9660 CD-ROM filesystem data ''
[devops@demo ~]$ cat helloworld.txt 
example
[devops@demo ~]$ sudo mount -t iso9660 test.iso /mnt/
mount: /mnt: WARNING: device write-protected, mounted read-only.
[devops@demo ~]$ ls -al /mnt/helloworld.txt 
-r-xr-xr-x. 1 root root 8 Aug 18 09:24 /mnt/virtualbox/helloworld.txt
[devops@demo ~]$ cat /mnt/helloworld.txt
example
[devops@demo ~]$
```

[](https://github.com/lucab85/ansible-pilot) [## GitHub-lucab 85/ansi ble-Pilot:ansi ble Pilot YouTube 频道代码库

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/lucab85/ansible-pilot) 

# 概述

现在您知道了如何从文件和文件夹创建 ISO 映像。

我希望你喜欢读这篇文章。如果你愿意支持我成为一名作家，考虑注册[成为一名媒体成员](https://ansiblepilot.medium.com/membership)。每月只需 5 美元，你就可以无限制地使用 Medium。

订阅 [YouTube 频道](https://www.youtube.com/channel/UC5MNbTYRHSCu9vAki3z9SmA)、 [Medium](https://ansiblepilot.medium.com/) 和[网站](https://www.ansiblepilot.com/)，不要错过 Ansible 试播的下一集。

# Ansible 的最佳资源

# 视频课程

*   [在 200 多个示例中学习 Ansible Automation&实践课程:通过一些关于如何使用最常见模块的真实示例和 ansi ble 剧本来学习 ansi ble](https://www.udemy.com/course/ansible-by-examples-devops/?referralCode=8E065F6D6F8622A3DEC8)

# 书

*   [可通过示例回答:针对 Linux 和 Windows 系统管理员和开发人员的 200 多个自动化示例](https://leanpub.com/ansiblebyexamples)
*   [负责 Windows 示例:针对 Windows 系统管理员和开发人员的 30 多个自动化示例](https://leanpub.com/ansibleforwindowsbyexamples)
*   [ansi ble For Linux by Examples:100 多个针对 Linux 系统管理员和开发人员的自动化示例](https://leanpub.com/ansibleforlinuxbyexamples)
*   [ansi ble Linux File system By Examples:30 多个针对现代 IT 基础设施的 Linux 文件和目录操作自动化示例](https://leanpub.com/linuxfileanddirectorybyansibleexamples)
*   [通过示例负责容器和 Kubernetes:10 多个自动化示例来自动化容器、Kubernetes 和 OpenShift](https://leanpub.com/ansible-for-kubernetes-by-examples)
*   [负责安全示例:100 多个自动化示例，用于自动化现代 IT 基础架构的安全和验证合规性](https://leanpub.com/ansibleforsecuritybyexamples)
*   [可行的技巧和窍门:10 多个可行的例子来节省时间和自动化更多的任务](https://leanpub.com/ansible-tips-and-tricks)
*   [Ansible Linux 用户&按示例分组:20 多个关于现代 IT 基础设施的 Linux 用户和分组操作的自动化示例](https://leanpub.com/ansiblelinuxusersandgroupsbyexamples)
*   [ansi ble For VMware by Examples:10 多个自动化您的 VMware 基础架构的示例(Ansible by Examples)](https://leanpub.com/ansible-for-vmware-by-examples)
*   [Ansible For PostgreSQL by Examples:10 多个自动化 PostgreSQL 数据库的示例](https://leanpub.com/ansible-for-postgresql-by-examples)
*   [ansi ble For Amazon Web Services AWS By Examples:10 多个自动化 AWS 现代基础设施的示例](https://leanpub.com/ansible-for-aws-by-examples)

# 捐赠

[](https://patreon.com/lucaberton) [## 卢卡·伯尔顿正在为 Ansible | Patreon 创建软件开源

### 今天就成为卢卡·伯尔顿的赞助人:获得世界上最大会员的独家内容和体验…

patreon.com](https://patreon.com/lucaberton) [](https://github.com/sponsors/lucab85) [## GitHub 赞助商上的赞助商@lucab85

### 我是一个活跃的开源贡献者，参与到了 Ansible 社区中，尽管我到处都是。@lucab85 的…

github.com](https://github.com/sponsors/lucab85) ![](img/8d33d5b9995ef8eaf1b5451e0b82b9a0.png)