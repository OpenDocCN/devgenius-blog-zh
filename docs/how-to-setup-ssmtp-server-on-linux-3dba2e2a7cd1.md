# 如何在 Linux 上设置 SSMTP 服务器

> 原文：<https://blog.devgenius.io/how-to-setup-ssmtp-server-on-linux-3dba2e2a7cd1?source=collection_archive---------1----------------------->

脚本和自动化

假设您被要求在服务器上自动执行每天、每周或每月的任务。为了确保任务成功完成，您希望您的自动化脚本向团队中的某些人或您自己发送电子邮件通知或报告。

![](img/6774f744c184c5a274d9b4368d1c92d0.png)

图片来源:[dribbble.com](https://dribbble.com/shots/2055709-Send-Email-GIF)

因此，本质上你是在尝试做以下事情之一:

*   从您的终端发送电子邮件
*   当服务器重新启动时得到通知
*   从服务器上运行的自动化脚本发送电子邮件通知
*   等等。

> 有各种各样的工具和库可以安装，以便从终端或运行在 Linux 服务器上的脚本发送电子邮件。

因此，在本教程中，我们将使用两个流行的库来演示如何在 Linux 机器上发送或接收电子邮件:

*   SSMTP
*   迈卢蒂尔

SMTP **(简单邮件传输协议)**或SMTP **(安全简单邮件传输协议)**用于**收发邮件**。它有时与其他库(如 Mailutil、IMAP 等)成对使用。它处理邮件的检索，而 SMTP 主要将邮件发送到服务器进行转发。

# 确认

首先，您希望通过运行以下命令来检查是否已经在该 Linux 机器上设置了 SMTP:

```
> mail -V> echo "Hello world" | mail -s "Test Email" your_email@gmail.com
```

检查 your_email@gmail.com 的收件箱或者你的垃圾邮件箱

如果您没有收到电子邮件或得到一个错误，那么 SMTP 没有设置，您可以继续设置。

# 安装库

使用以下命令安装 Mailutils 和 SSMTP 库软件包:

```
> apt-get install mailutils
```

*   在安装过程中→保留所有默认设置并选择“无配置”

```
> apt-get install ssmtp
```

成功安装 Mailutils 和 SSMTP 后，验证并确保发送邮件所需的所有全局配置都设置正确:

*   检查邮件工具→邮件-V
*   打开并编辑以下文件:

```
> nano /etc/ssmtp/ssmtp.conf
```

编辑上面的文件，并确保 M ***ailhub*** 设置为您的 SMTP 服务器。

如果您想要 Gmail 的 SMTP 服务器，您需要调整要使用的 Gmail 帐户的安全设置。进入[谷歌账户设置](https://www.google.com/settings/u/1/security)，启用选项**允许不太安全的应用**，默认关闭:

那么您的 ssmtp.conf 内容应该包含以下内容:

*   **发送邮件服务器(SMTP 服务器)或 mailhub:** `smtp.gmail.com`
*   **用户名:**您的 Gmail 帐户 ID(例如，如果您的电子邮件是`your_email@gmail.com`，则为`your_email`
*   **密码:**您的 Gmail 密码
*   **使用认证:**是
*   **使用安全连接:**是
*   **端口:** `587` (TLS)或`465` (SSL)

如果您使用的是自己的 SMTP 服务器，那么按如下方式设置您的*邮件中心*:

```
mailhub=your.ssmtpserver.example:port_used
```

和

```
FromLineOverride=YES
```

现在，您可以从命令行发送电子邮件了。通过再次运行以下命令来测试它:

```
> echo "Hello Friend" | mail -s "Test Email" your_friend_email@gmail.com
```

> ***如果出现错误，请查找、调查并排除故障***

# 从脚本发送电子邮件

如果您正在设置某种自动化脚本，并希望在脚本运行或任务完成时发送电子邮件通知；
您也可以使用相同的 SMTP 从 shell 脚本发送邮件。您的代码可能如下所示:

> 如果你喜欢这篇文章，你可能也会喜欢； [Cron & logrotate:如何使用 Cron 自动化任务](/cron-logrotate-how-to-use-cron-to-automate-tasks-a93069d9185c)

> 干杯！！！