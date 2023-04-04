# 如何在本地机器上设置 SSH 进行源代码管理

> 原文：<https://blog.devgenius.io/how-to-setup-ssh-for-source-code-management-to-local-machine-d805bb2ed28b?source=collection_archive---------9----------------------->

这适用于 GitLab、GitHub 和 Bitbucket

![](img/93190dd42a5f2f3a733b136ac270bdb6.png)

图片来源:[www.hostinger.com](https://www.hostinger.com/tutorials/ssh/how-to-set-up-ssh-keys)

1.  对于 Windows →和 cmd 一样去“Git Bash”。右键单击并“以管理员身份运行”。对于 Mac 和 Linux →转到终端。
2.  类型`ssh-keygen`
3.  按回车键。
4.  它会要求您将密钥保存到特定的目录。
5.  按回车键。它会提示您键入密码或按 ***输入*** 而不输入密码。
6.  将为指定的目录创建公钥。
7.  现在转到目录并打开`.ssh`文件夹。
8.  你会看到一个文件`id_rsa.pub`。在记事本或任何你喜欢的 IDE 上打开它。从中复制所有文本。
9.  转到您的 GitHub、Bitbucket 或 GitLab 配置文件→例如[gitlab.com/profile/keys](https://gitlab.com/profile/keys)。
10.  粘贴到“密钥”文本字段。
11.  现在点击下面的“标题”。它会自动填满。
12.  然后点击“添加密钥”。

> **现在用 SSH 尝试一个 git 克隆存储库，它会像预期的那样工作。**

> 干杯！！