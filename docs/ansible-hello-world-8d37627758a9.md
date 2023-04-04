# 你好，世界

> 原文：<https://blog.devgenius.io/ansible-hello-world-8d37627758a9?source=collection_archive---------2----------------------->

## **入门指南**

![](img/cd5ce478a6a14770ce66fc53ca7e439b.png)

**等级——初级**

**Ansible** 是一款软件配置管理工具，用于在不同类型的云托管+传统数据中心上自动化基础设施供应。它基于 **ssh 连接**工作，因此不需要任何代理设置。**无代理**特性与 **yaml** 语法的易用性相结合，使其成为开发人员/操作人员社区中的流行语言。

从 **Ansible** 常用术语开始阅读的好材料可以在这里找到:[备忘单](https://www.digitalocean.com/community/cheatsheets/how-to-use-ansible-cheat-sheet-guide)。我们将在以后的博客中讨论该架构，并与其他可用的自动化工具进行比较。

# 我的第一次 Nginx 部署

下面详细介绍了使用 ansible 在远程服务器上设置 nginx 部署的步骤。在本教程中，我们将使用 *MacOSX* 作为操作系统。

# 先决条件

**Python:**

```
brew install python3
```

可回答的:

```
brew install ansible
```

**虚拟机:**

公共/私有云上的虚拟机，使用 ssh 私有/公共密钥在端口 22 上进行访问。

# 安装步骤:

按照步骤，用户可以克隆[https://github.com/amit894/ansible-examples](https://github.com/amit894/ansible-examples)。我们将在代码库中创建一个类似于 ***nginx-vm*** 的剧本。

## **i)更新主机文件**

在根目录下，创建一个名为 **hosts** 的清单文件

```
[server-name]
<vm-ip> ansible_user=<ssh_user_name> ansible_ssh_private_key_file=<location of_private_key>
```

## **ii)创建剧本/角色文件:**

使用命令添加角色

```
ansible-galaxy init <role-name>
```

角色可以在剧本文件和宿主文件中被引用，如下所示:

```
- name: <role-name>
  hosts: <server-name>
  roles:
    - role: <role-folder-location>
```

## **iii)添加任务:**

每个可承担的角色可以分为四个基本的设置任务，用于模块的逻辑分离:

**先决条件**:更新虚拟机的 yum/apt-get 存储库。
**安装**:包括为角色安装二进制文件的步骤。
**配置:**包括配置设置所需路径/目录的步骤。
**服务**:包括设置服务的步骤，如*服务重启、配置测试*等。

可以为 nginx 安装角色中的任务克隆存储库中的文件。所有这些任务都可以在 main.yml 中引用，如下所示。

```
---
# tasks file for nginx
- import_tasks: prereqs.yml
- import_tasks: install.yml
- import_tasks: configure.yml
- import_tasks: service.yml
```

## **iv)配置模板文件:**

任何需要模板化的附加文件都可以放在 ***角色/模板*** 文件夹中。对于 nginx 的安装，其中一些文件可以是 index.html 的 ***、nginx.conf 的*** 等。

可以为 nginx 安装角色中的任务克隆存储库中的文件

## 测试你的代码:

```
ansible-playbook ./playbooks/<playbook-name> -i hosts
```

# 反馈:

我们正试图为 devops 领域的初学者创建基本内容。因此，请将您的反馈发送至以下任一邮箱:

I)媒体中的评论
ii) Github 知识库中的问题
iii)发送电子邮件至***Amit[dot]894【at】Gmail[dot]com***