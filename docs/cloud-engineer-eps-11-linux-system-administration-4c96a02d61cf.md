# 云工程师 Eps。11: Linux —系统管理

> 原文：<https://blog.devgenius.io/cloud-engineer-eps-11-linux-system-administration-4c96a02d61cf?source=collection_archive---------18----------------------->

## 用户管理

![](img/1282510876274312650973f569bdf771.png)

# 简短介绍

在一个管理环境中，最关键的事情之一是**用户管理**。从那里，我们可以为公司的员工添加对公司应用程序的访问，为每个部门创建特定的策略，以及许多其他事情。在这篇文章中，我将向你展示我在**用户管理**方面学到的基础知识。

# USERADD & USERDEL

## 例子

如果我们深入到命令本身，有两个词:`del`和`add`。所以我想我们已经知道这个命令的用途了。

```
Column 1: USERADD & USERDEL command.Format:
useradd [args] [username]**useradd [username]**
//This will create [username] with default options that listed in etc/login.defs, where System will create home directory for [username] and random [user id].**useradd -m -e YYYY-MM-DD [username] -g NNNN**
//command **-m** is basically not used, because by default, the system will create new account's directory, -e means for expiry dates of the account, **-g** means put the account to the group chosen (we should declare this by number or **Group ID** not by its name)**userdel -f [username]** //this command will delete [username] with its folder by using options **-f**, without that, the home folder will remains.
```

## 选择

下面是 Linux 中这两个命令的可用选项。

## 检查用户列表

您可以通过`cat /etc/passwd`在用户列表中轻松查看已创建的用户。

## USERMOD

这是与 **useradd** 相关的命令。我们将用这个来管理我们的用户。

```
Column 2: USERMOD command.Format:
usermod [args] [username]**usermod -e YYYY-MM-DD [username]**
//This will change expiration date of account of [username]**usermod -d [username after] [username before]** //This will move all [username after] contents in its directory to [username before] directory**usermod -p [P@ssw0rd$] [username]** //This will create new password for [username]**usermod -L [username]** //This will lock the account**usermod -U [username]** //This will unlock the account**usermod -l [username after] [username before]**
//This will change login name to [username after] for [username before]
```

# GROUPADD & GROUPDEL

## 例子

```
Column 3: GROUPADD & GROUPDEL command.Format:
groupadd [args] [username]**groupadd -f -g NNNN [groupname]**
//This will create new [groupname] and return success if exist & cancel **-g** if it has been used because of **-f****groupadd -p [P@ssw0rd$] -r [groupname]** //This will create new [groupname] with [P@ssw0rd$] written and making it a root group because of **-r****groupdel -f [groupname]** //This will delete [groupname] even if it is a primary group of a user
```

## 选择

以下是这些命令可用的选项。

## 检查用户列表

您可以通过`cat /etc/group`在用户列表中轻松查看已创建的用户。

## GROUPMOD

```
Column 4: GROUPMOD command.Format:
groupmod [args] [username]**groupmod -g NNNN [groupname]**
//This will change GID (Group ID) of [groupname]**groupmod -n [new name] [groupname]**
//This will change [groupname] to [new name]**groupmod -p [P@ssw0rd$] [groupname]**
//This will change the password of the [groupname]
```

# 结论

祝贺我们已经知道这些命令用于用户管理。这不是终点。我将在下一篇文章中继续讨论**用户管理**。再见！

```
 Read more of my stories Here!
[< My Previous Blog](/cloud-engineer-eps-10-linux-system-administration-e6872112936f)                                    [My Next Blog >](https://medium.com/@sayful.adrian/a-love-note-from-me-e751a9671789)
```