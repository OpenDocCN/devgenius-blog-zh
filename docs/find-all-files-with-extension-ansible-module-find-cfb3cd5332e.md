# 查找所有带扩展名的文件— Ansible 模块查找

> 原文：<https://blog.devgenius.io/find-all-files-with-extension-ansible-module-find-cfb3cd5332e?source=collection_archive---------2----------------------->

## 如何自动查找所有文件？使用 Ansible Playbook 和 find 模块的“devops”用户的“示例”目录下的“cnf”扩展。

# 如何用 Ansible 找到所有特定扩展名的文件？

我将向您展示一个带有一些简单的 Ansible 代码的现场演示。我是卢卡·伯尔顿，欢迎来到今天的《神秘飞行员》节目。

Ansible 查找所有带扩展名的文件

*   `ansible.builtin.find`
*   根据特定条件返回文件列表

今天我们讨论的是 Ansible 模块“`find`”。

全名是``ansible.builtin.find`,这意味着它是``ansible-core`内置集合中包含的集合的一部分。

这个模块使用流行的 Unix 命令``find`'返回基于特定标准的文件列表。

# 因素

*   **路径**字符串—要搜索的目录的路径列表
*   隐藏布尔值—否/是
*   递归布尔值—递归下降到目录中查找文件
*   文件类型字符串— **文件**/目录/任意/链接
*   模式列表—搜索(外壳或正则表达式)模式
*   use _ regex boolean 否/是-文件全局(shell) / python 正则表达式

对于此用例,“查找”模块的最重要参数。

强制参数``paths`'指定要搜索的目录的路径列表。

您可以用``hidden`'参数包含隐藏文件。

以及使用``recurse`'参数在主路径下的任何目录中递归。

另一个有用的参数是“`file_type`”,默认为“`file`”,但是您可以过滤“`directory`”、“`link`”或“`any`”文件系统对象类型。

在“`patterns`”列表下指定要搜索的内容。

Ansible 默认使用文件 globs (shell)模式，但是您也可以指定 python 正则表达式来启用``use_regex`'参数。

# 链接

*   [https://docs . ansi ble . com/ansi ble/latest/collections/ansi ble/builtin/find _ module . html](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/find_module.html)

演示

如何用 Ansible Playbook 查找所有带扩展名的文件？

我将只搜索我的登录用户(`devops`)的示例文件夹下的文件和目录，并列出所有扩展名为“`.cnf`”的文件。

该代码对目标机器没有危险的影响。

## 密码

```
---
- name: find demo
  hosts: all
  vars:
    mypath: "/home/devops/example"
    mypattern: '*.cnf'
  tasks:
    - name: search files
      ansible.builtin.find:
        paths: "{{ mypath }}"
        hidden: true
        recurse: true
        file_type: any
        patterns: "{{ mypattern }}"
      register: found_files - name: print files
      ansible.builtin.debug:
        var: found_files
```

## 执行

```
$ ansible-playbook -i virtualmachines/demo/inventory file_management/file_find.ymlPLAY [find demo] **********************************************************************************TASK [Gathering Facts] ****************************************************************************
ok: [demo.example.com]TASK [search files] *******************************************************************************
ok: [demo.example.com]TASK [print files] ********************************************************************************
ok: [demo.example.com] => {
    "found_files": {
        "changed": false,
        "examined": 3,
        "failed": false,
        "files": [
            {
                "atime": 1657183703.9653573,
                "ctime": 1657183703.9653573,
                "dev": 64768,
                "gid": 10,
                "gr_name": "wheel",
                "inode": 134965918,
                "isblk": false,
                "ischr": false,
                "isdir": false,
                "isfifo": false,
                "isgid": false,
                "islnk": false,
                "isreg": true,
                "issock": false,
                "isuid": false,
                "mode": "0644",
                "mtime": 1657183703.9653573,
                "nlink": 1,
                "path": "/home/devops/example/file1.cnf",
                "pw_name": "devops",
                "rgrp": true,
                "roth": true,
                "rusr": true,
                "size": 0,
                "uid": 1001,
                "wgrp": false,
                "woth": false,
                "wusr": true,
                "xgrp": false,
                "xoth": false,
                "xusr": false
            },
            {
                "atime": 1657183705.7742639,
                "ctime": 1657183705.7742639,
                "dev": 64768,
                "gid": 10,
                "gr_name": "wheel",
                "inode": 134965931,
                "isblk": false,
                "ischr": false,
                "isdir": false,
                "isfifo": false,
                "isgid": false,
                "islnk": false,
                "isreg": true,
                "issock": false,
                "isuid": false,
                "mode": "0644",
                "mtime": 1657183705.7742639,
                "nlink": 1,
                "path": "/home/devops/example/file2.cnf",
                "pw_name": "devops",
                "rgrp": true,
                "roth": true,
                "rusr": true,
                "size": 0,
                "uid": 1001,
                "wgrp": false,
                "woth": false,
                "wusr": true,
                "xgrp": false,
                "xoth": false,
                "xusr": false
            }
        ],
        "matched": 2,
        "msg": "All paths examined",
        "skipped_paths": {}
    }
}PLAY RECAP ****************************************************************************************
demo.example.com           : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## 幂等性

```
$ ansible-playbook -i virtualmachines/demo/inventory file_management/file_find.ymlPLAY [find demo] **********************************************************************************TASK [Gathering Facts] ****************************************************************************
ok: [demo.example.com]TASK [search files] *******************************************************************************
ok: [demo.example.com]TASK [print files] ********************************************************************************
ok: [demo.example.com] => {
    "found_files": {
        "changed": false,
        "examined": 3,
        "failed": false,
        "files": [
            {
                "atime": 1657183703.9653573,
                "ctime": 1657183703.9653573,
                "dev": 64768,
                "gid": 10,
                "gr_name": "wheel",
                "inode": 134965918,
                "isblk": false,
                "ischr": false,
                "isdir": false,
                "isfifo": false,
                "isgid": false,
                "islnk": false,
                "isreg": true,
                "issock": false,
                "isuid": false,
                "mode": "0644",
                "mtime": 1657183703.9653573,
                "nlink": 1,
                "path": "/home/devops/example/file1.cnf",
                "pw_name": "devops",
                "rgrp": true,
                "roth": true,
                "rusr": true,
                "size": 0,
                "uid": 1001,
                "wgrp": false,
                "woth": false,
                "wusr": true,
                "xgrp": false,
                "xoth": false,
                "xusr": false
            },
            {
                "atime": 1657183705.7742639,
                "ctime": 1657183705.7742639,
                "dev": 64768,
                "gid": 10,
                "gr_name": "wheel",
                "inode": 134965931,
                "isblk": false,
                "ischr": false,
                "isdir": false,
                "isfifo": false,
                "isgid": false,
                "islnk": false,
                "isreg": true,
                "issock": false,
                "isuid": false,
                "mode": "0644",
                "mtime": 1657183705.7742639,
                "nlink": 1,
                "path": "/home/devops/example/file2.cnf",
                "pw_name": "devops",
                "rgrp": true,
                "roth": true,
                "rusr": true,
                "size": 0,
                "uid": 1001,
                "wgrp": false,
                "woth": false,
                "wusr": true,
                "xgrp": false,
                "xoth": false,
                "xusr": false
            }
        ],
        "matched": 2,
        "msg": "All paths examined",
        "skipped_paths": {}
    }
}PLAY RECAP ****************************************************************************************
demo.example.com           : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## 执行前

```
$ ssh [devops@demo.example.com](mailto:devops@demo.example.com)
[devops@demo ~]$ cd example
[devops@demo example]$ ls
file1.cnf  file2.cnf  file3.txt
[devops@demo example]$ tree
.
|-- file1.cnf
|-- file2.cnf
`-- file3.txt0 directories, 3 files
[devops@demo example]$
```

## 执行后

```
$ ssh [devops@demo.example.com](mailto:devops@demo.example.com)
[devops@demo ~]$ cd example
[devops@demo example]$ ls
file1.cnf  file2.cnf  file3.txt
[devops@demo example]$ tree
.
|-- file1.cnf
|-- file2.cnf
`-- file3.txt
0 directories, 3 files
[devops@demo example]$
```

[](https://github.com/lucab85/ansible-pilot) [## GitHub-lucab 85/ansi ble-Pilot:ansi ble Pilot YouTube 频道代码库

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/lucab85/ansible-pilot) 

# 概述

现在你知道如何用 Ansible 查找所有带扩展名的文件了。

我希望你喜欢读这篇文章。如果你愿意支持我成为一名作家，考虑注册[成为一名媒体成员](https://ansiblepilot.medium.com/membership)。每月只需 5 美元，你就可以无限制地使用 Medium。

订阅 [YouTube 频道](https://www.youtube.com/channel/UC5MNbTYRHSCu9vAki3z9SmA)、 [Medium](https://ansiblepilot.medium.com/) 和[网站](https://www.ansiblepilot.com/)，不要错过 Ansible 试播的下一集。

# Ansible 的最佳资源

## 视频课程

*   [通过 200 多个示例学习 Ansible Automation&实践课程:通过一些真实的例子学习 Ansible，了解如何使用最常见的模块和 ansi ble 行动手册](https://www.udemy.com/course/ansible-by-examples-devops/?referralCode=8E065F6D6F8622A3DEC8)

## 书

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

github.com](https://github.com/sponsors/lucab85) ![](img/4f83934d1860f593ecf063ecaf28136b.png)