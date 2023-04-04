# 在 Linux 上使用 Crontab 的技巧和诀窍

> 原文：<https://blog.devgenius.io/tips-and-tricks-for-using-crontab-on-linux-15d5ecb323e3?source=collection_archive---------4----------------------->

![](img/cf110d0e43d183e97a2afcb70e989538.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由[Towfiqu barb huya](https://unsplash.com/@towfiqu999999?utm_source=medium&utm_medium=referral)拍摄的照片

Crontab 确实很有用，但是也有一些问题。以下都是在 Ubuntu 服务器上完成的。在其他发行版上，结果可能会有所不同。

**设置 Cron 作业**

快跑吧

```
crontab -e
```

文件将在默认编辑器中打开，第一个列表被注释掉。这些评论是有用的。我在这里复制了前几个:

```
# Edit this file to introduce tasks to be run by cron.
#
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
```

您最终会添加一行，其格式类似于:

```
@hourly /home/tony/dev/website/utils/digOceanDynamicIp.sh
```

这是一个每小时运行一次的脚本。这个脚本更新我的服务器在 DigitalOceans DNS 上的公共 IP 地址(如果它改变了)。

## **调度**

注意上面的 *@hourly* 。这是一种简写的非标准格式。有很多简写选项，包括*@每年、@每年、@每月、@每周、@每天、@每小时、*和*@重新启动。*

调度的标准格式表示命令的调度，在要运行的命令之前有五个令牌。

例如，每小时的标准格式是:

```
0 * * * *
```

从技术上来说，这可以解释为“每小时、每天、每月的零时运行”。

老实说，用 [Crontab Guru](https://crontab.guru/) 就行了。太牛逼了。它将帮助您学习日程安排符号，并使您轻松设置所需的日程。

## 为了 Sudo 特权

如果您的脚本需要 *sudo* 权限，最好使用 *sudo* crontab，通过运行

```
sudo crontab -e
```

# **Crontab 输出**

## **脚本输出**

我经常喜欢使用 crontab 来安排 bash 脚本。脚本通常会将一些信息输出到标准输出中。

然后在 crontab 中，可以将输出定向到日志文件。

举个例子，

```
30 4 * * * /home/tony/toSomethingUsefulAndPrintOutput.sh > /home/tony/logs/outputOfUsefulThing.log
```

将脚本输出到文件*/home/Tony/logs/outputofusefulthing . log*

## **命令输出**

在默认安装中，cron 作业被记录到 */var/log/syslog*

通过运行以下命令，您可以在该日志文件中只看到 cron 作业

```
grep CRON /var/log/syslog
```

您可能会看到如下错误

```
Sep 14 02:00:01 tonyserver CRON[2944396]: (CRON) info (No MTA installed, discarding output)
```

这是因为 crontab 实际上试图发送每次运行的输出。我知道，这是很古老的学校。

Ubuntu Server 默认没有安装邮件客户端。

您可以通过安装 postfix(或者您喜欢的其他邮件客户端)并将其配置为在本地保存文件来解决这个问题。

```
sudo apt install postfix
```

要在安装过程中使用带有 cron 的 postfix(如果您不想实际向外发送电子邮件),您应该回答配置为仅用于本地。这意味着不使用网络，邮件只是由邮件客户端保存在本地。

在 Ubuntu 服务器上，存储输出的位置最终是 */var/mail/ <用户>*

更多关于安装 postfix 的信息，请看[这里](https://askubuntu.com/questions/222512/cron-info-no-mta-installed-discarding-output-error-in-the-syslog)。

## **测试**

设置完成后，您可以通过安排 cron 任务在接下来的几分钟内运行并跟踪日志来进行测试。如果你已经确保你的脚本是等幂的，那么测试就容易一些。

然后，只需使用调度字符串将其设置为每 2 分钟运行一次:

```
*/2 * * * *
```

并在输出的日志后面加上

```
sudo tail -f /var/mail/<user>
```

## **需要注意的额外问题**

**如果 cron 作业正在访问 GitHub** 并且您正在运行 *sudo* crontab，那么存储在非根用户路径中的 ssh 密钥将不起作用。需要为根用户创建一个新的密钥。

奔跑

```
sudo ssh-keygen
```

并将生成的公钥存储在您的 Github 帐户中。

有关设置 ssh 密钥的更多信息，请参见[https://www . digital ocean . com/community/tutorials/how-to-set-up-ssh-keys-on-Ubuntu-1604](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-1604)。