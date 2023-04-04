# 可行的故障排除—目的地不存在 rc 257

> 原文：<https://blog.devgenius.io/ansible-troubleshooting-destination-does-not-exist-rc-257-4679eb5d0b72?source=collection_archive---------13----------------------->

## 如何解决错误“目标不存在”，返回代码 257 使用 Ansible lineinfile 模块为 SSH 启用 PasswordAuthentication。

今天我们将讨论 Ansible 故障排除，特别是关于“目的地不存在”消息，“返回代码 257”。

当我们试图编辑目标文件系统上不存在的文件时，会出现这个致命的错误消息。根本原因可能是文件拼写错误或根本不存在。

这些情况通常与可行的剧本或可行的配置有关。

我是卢卡·伯尔顿，欢迎来到今天的《神秘飞行员》节目。

# 演示

谈论 Ansible 故障排除的最佳方式是进行现场演示，向您实际展示“目的地不存在”，返回代码 257，以及如何解决它！

这个演示将尝试在我们的目标系统上编辑一个拼写错误的配置文件“/etc/ssh/sshd_config”。

## 错误代码

```
---
- name: PasswordAuthentication enabled
  hosts: all
  become: true
  gather_facts: false
  tasks:
    - name: ssh PasswordAuthentication
      ansible.builtin.lineinfile:
        dest: **/etc/ssh/sshd_config2**
        regexp: '^PasswordAuthentication'
        line: "PasswordAuthentication yes"
        state: present
      notify: ssh restart handlers:
    - name: ssh restart
      ansible.builtin.service:
        name: sshd
        state: restarted
```

## 错误执行

```
$ ansible-playbook -i virtualmachines/demo/inventory edit\ single-line\ text/enable_password_auth.ymlPLAY [PasswordAuthentication enabled] *************************************************************TASK [ssh PasswordAuthentication] *****************************************************************
fatal: [demo.example.com]: FAILED! => {"ansible_facts": {"discovered_interpreter_python": "/usr/libexec/platform-python"}, "changed": false, "msg": "Destination **/etc/ssh/sshad_config2** does not exist !", "rc": 257}PLAY RECAP ****************************************************************************************
demo.example.com           : ok=0    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
```

## 修复代码

```
---
- name: PasswordAuthentication enabled
  hosts: all
  become: true
  gather_facts: false
  tasks:
    - name: ssh PasswordAuthentication
      ansible.builtin.lineinfile:
        dest: **/etc/ssh/sshd_config**
        regexp: '^PasswordAuthentication'
        line: "PasswordAuthentication yes"
        state: present
      notify: ssh restart handlers:
    - name: ssh restart
      ansible.builtin.service:
        name: sshd
        state: restarted
```

## 修复执行

```
$ ansible-playbook -i virtualmachines/demo/inventory edit\ single-line\ text/enable_password_auth.ymlPLAY [PasswordAuthentication enabled] *************************************************************TASK [ssh PasswordAuthentication] *****************************************************************
changed: [demo.example.com]RUNNING HANDLER [ssh restart] *********************************************************************
changed: [demo.example.com]PLAY RECAP ****************************************************************************************
demo.example.com           : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## 确认

```
$ ssh [devops@demo.example.com](mailto:devops@demo.example.com)
[devops@demo ~]$ sudo su
[root@demo devops]# grep 'PasswordAuthentication yes' /etc/ssh/sshd_config 
#PasswordAuthentication yes
PasswordAuthentication yes
[root@demo devops]# systemctl status sshd
● sshd.service - OpenSSH server daemon
   Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2022-07-11 08:49:05 UTC; 56s ago
     Docs: man:sshd(8)
           man:sshd_config(5)
 Main PID: 5106 (sshd)
    Tasks: 1 (limit: 4952)
   Memory: 2.8M
   CGroup: /system.slice/sshd.service
           └─5106 /usr/sbin/sshd -D -u0 -[oCiphers=aes256-gcm@openssh.com](mailto:oCiphers=aes256-gcm@openssh.com),chacha20-poly1305@openssh>Jul 11 08:49:05 demo.example.com systemd[1]: sshd.service: Succeeded.
Jul 11 08:49:05 demo.example.com systemd[1]: Stopped OpenSSH server daemon.
Jul 11 08:49:05 demo.example.com systemd[1]: Starting OpenSSH server daemon...
Jul 11 08:49:05 demo.example.com sshd[5106]: Server listening on 0.0.0.0 port 22.
Jul 11 08:49:05 demo.example.com sshd[5106]: Server listening on :: port 22.
Jul 11 08:49:05 demo.example.com systemd[1]: Started OpenSSH server daemon.
Jul 11 08:49:29 demo.example.com sshd[5123]: Accepted publickey for devops from 192.168.43.5 port >
Jul 11 08:49:29 demo.example.com sshd[5123]: pam_unix(sshd:session): session opened for user devop>
[root@demo devops]#
```

[](https://github.com/lucab85/ansible-pilot) [## GitHub-lucab 85/ansi ble-Pilot:ansi ble Pilot YouTube 频道代码库

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/lucab85/ansible-pilot) 

# 概述

现在您更好地了解了如何对 ansi ble“Destination is not exist”消息(返回代码 257)进行故障诊断。

我希望你喜欢读这篇文章。如果你愿意支持我成为一名作家，考虑注册[成为一名媒体成员](https://ansiblepilot.medium.com/membership)。每月只需 5 美元，你就可以无限制地使用 Medium。

订阅 [YouTube 频道](https://www.youtube.com/channel/UC5MNbTYRHSCu9vAki3z9SmA)、 [Medium](https://ansiblepilot.medium.com/) 和[网站](https://www.ansiblepilot.com/)，不要错过 Ansible 试播的下一集。

# Ansible 的最佳资源

# 视频课程

*   [通过 200 多个示例学习 Ansible Automation&实践课程:通过一些真实的例子学习 Ansible，了解如何使用最常见的模块和 ansi ble 行动手册](https://www.udemy.com/course/ansible-by-examples-devops/?referralCode=8E065F6D6F8622A3DEC8)

# 书

*   [可通过示例回答:针对 Linux 和 Windows 系统管理员和开发人员的 200 多个自动化示例](https://leanpub.com/ansiblebyexamples)
*   [适用于 Windows 的示例:针对 Windows 系统管理员和开发人员的 30 多个自动化示例](https://www.amazon.com/dp/B09VCTP1L4)
*   [ansi ble For Linux by Examples:100 多个针对 Linux 系统管理员和开发人员的自动化示例](https://www.amazon.com/dp/B09VCX4CVD)
*   [Ansible Linux 文件系统示例:30 多个针对现代 IT 基础设施的 Linux 文件和目录操作自动化示例](https://www.amazon.com/dp/B09X4TF8P8)
*   [通过示例负责容器和 Kubernetes:10 多个自动化示例来自动化容器、Kubernetes 和 OpenShift](https://www.amazon.com/dp/B09XVDWD4W)
*   [负责安全示例:100 多个自动化示例，用于自动化现代 IT 基础设施的安全和验证合规性](https://www.amazon.com/dp/B09VHFLSPP)
*   [可行的技巧和窍门:10 多个可行的例子来节省时间和自动化更多的任务](https://www.amazon.com/dp/B09ZJ4GYM2)
*   [Ansible Linux 用户&按示例分组:20 多个关于现代 IT 基础设施的 Linux 用户和分组操作的自动化示例](https://www.amazon.com/dp/B09X6BYY8W)
*   [ansi ble For VMware by Examples:10 多个自动化您的 VMware 基础架构的示例(Ansible by Examples)](https://www.amazon.com/dp/B0B18QG44S)
*   [Ansible For PostgreSQL by Examples:10 多个自动化 PostgreSQL 数据库的示例](https://www.amazon.com/dp/B0B3RZZQVG)

# 捐赠

[](https://patreon.com/lucaberton) [## 卢卡·伯尔顿正在为 Ansible | Patreon 创建软件开源

### 今天就成为卢卡·伯尔顿的赞助人:获得世界上最大会员的独家内容和体验…

patreon.com](https://patreon.com/lucaberton) [](https://github.com/sponsors/lucab85) [## GitHub 赞助商上的赞助商@lucab85

### 我是一个活跃的开源贡献者，参与到了 Ansible 社区中，尽管我到处都是。@lucab85 的…

github.com](https://github.com/sponsors/lucab85) ![](img/376f5c3eb35945e4fb9ac55ff704fe0d.png)