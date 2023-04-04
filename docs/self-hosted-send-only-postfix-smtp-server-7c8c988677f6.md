# 自托管仅发送 Postfix SMTP 服务器

> 原文：<https://blog.devgenius.io/self-hosted-send-only-postfix-smtp-server-7c8c988677f6?source=collection_archive---------5----------------------->

![](img/4a9d62cfdaf48c87baf932a44ae7d77a.png)

我需要为我的新项目发电子邮件。我首先探索了 SaaS 服务。他们提供的免费套餐有少量发送邮件。所以只要我是程序员，我就决定托管自己的发送邮件服务器。

经过一些研究，我决定继续使用 Postfix 只发送配置。继续阅读如何安装和配置 SMTP 服务器来发送电子邮件，而不是将它们发送到垃圾邮件文件夹。

# 先决条件

我用的是单核 VPS 和 Ubuntu 18.04，每月只需 2.5 美元。我的主应用程序使用 DB 中的一个表作为电子邮件队列，所以我有两个 NodeJS 应用程序:一个处理队列，另一个获取交付状态并在主应用程序 DB 中更新它。如果你想看就告诉我。这里只是后缀部分。

我也有一个域名。让它在这里成为 example.com。因为它将被用于一个网站，我用一个子域【email.example.com 作为邮件服务器。

# 后缀安装

使用以下命令安装 Postfix:

```
sudo apt-get update
sudo apt install mailutils
```

安装时，选择**网站**选项进行邮件类型配置。作为**系统邮件名**输入你的域名，例如 example.com。
稍后您可以使用以下命令查看域:

```
cat /etc/mailname
```

# 服务器配置

用编辑器打开主配置文件:

```
sudo nano /etc/postfix/main.cf
```

找到 **inet_interfaces** 参数，将其更改为**仅环回**。使用该参数，Postfix 将不会侦听来自 VPS 外部的任何连接。

```
inet_interfaces = loopback-only
```

现在要更改的其他参数和值:

```
mydestination = $myhostname, localhost.$mydomain, localhost
```

将您的邮件域设置为服务器主机名:

```
sudo hostnamectl set-hostname email.example.com
```

使用命令检查主机名:

```
hostname --f
```

编辑 **/etc/hosts** 文件，并使用您的远程 IP 地址添加该子域:

```
1.2.3.4    email.example.com email
```

配置更改后重新启动 Postfix 的命令:

```
sudo systemctl restart postfix
```

检查当前配置的命令:

```
postconf -d
```

# 创建 DKIM 签名

安装 DKIM 工具:

```
sudo apt-get install opendkim opendkim-tools
```

将用户添加到组中:

```
sudo gpasswd -a postfix opendkim
```

打开配置文件/etc/opendkim.conf:

```
sudo nano /etc/opendkim.conf
```

然后在配置文件中添加或更新以下参数:

```
Socket              inet:8892@localhost
Canonicalization    simple
Mode                sv
SubDomains          no
AutoRestart         yes
AutoRestartRate     10/1M
Background          yes
DNSTimeout          5
SignatureAlgorithm  rsa-sha256
UserID              opendkim
KeyTable            refile:/etc/opendkim/key.table
SigningTable        refile:/etc/opendkim/signing.table
ExternalIgnoreList  /etc/opendkim/trusted.hosts
InternalHosts       /etc/opendkim/trusted.hosts
```

为 DKIM 密钥创建文件夹:

```
sudo mkdir -p /etc/opendkim/keys
```

更改文件夹权限:

```
sudo chown -R opendkim:opendkim /etc/opendkim
sudo chmod go-rw /etc/opendkim/keys
```

创建 opendkim 签名表:

```
sudo nano /etc/opendkim/signing.table
```

里面有内容:

```
*@example.com    default._domainkey.example.com
```

然后创建密钥表:

```
sudo nano /etc/opendkim/key.table
```

里面有内容:

```
default._domainkey.example.com     example.com:default:/etc/opendkim/keys/example.com/default.private
```

添加受信任的主机:

```
sudo nano /etc/opendkim/trusted.hosts
```

里面有内容:

```
127.0.0.1
localhost
*.example.com
```

创建密钥文件夹:

```
sudo mkdir /etc/opendkim/keys/example.com
```

然后生成密钥:

```
sudo opendkim-genkey -b 2048 -d example.com -D /etc/opendkim/keys/example.com -s default -v
```

更改私钥文件所有者:

```
sudo chown opendkim:opendkim /etc/opendkim/keys/example.com/default.private
```

然后重新启动 opendkim 服务:

```
sudo service opendkim restart
```

检查密钥:

```
sudo opendkim-testkey -d example.com -s default -vvv
```

现在在 Postfix 配置文件(/etc/postfix/main.cf)中添加或更新参数:

```
milter_default_action = accept
milter_protocol = 2
smtpd_milters = inet:localhost:8892
non_smtpd_milters = inet:localhost:8892
```

然后重新启动 Postfix:

```
sudo systemctl restart postfix
```

# DNS 配置

您需要配置一些 DNS 记录。通过这种配置，电子邮件服务不会将您的电子邮件视为垃圾邮件。

为 email.example.com 添加记录:

```
Type: A
Name: email.example.com
Value: 1.2.3.4
```

添加 SPF 记录:

```
Type: TXT
Name: example.com
Value: v=spf1 mx ~all
```

添加 DMARC 记录:

```
Type: TXT
Name: _dmarc
Value: v=DMARC1; p=none
```

添加 DKIM 记录。使用以前生成的 DKIM 签名(/etc/open DKIM/keys/example . com/default . txt):

```
Type: TXT
Name: default._domainkey
Value: v=DKIM1; h=sha256; k=rsa; p=you-key-here-without-spaces
```

设置 PTR 记录(rDNS)。你需要在你的主机提供商的控制面板中设置它。将 email.example.com 设置为主机名，将 4.3.2.1.in-addr.arpa 设置为 IP 地址。IP 应该是相反的顺序。
使用以下命令检查其设置是否正确:

```
host 1.2.3.4
```

# 电子邮件加密

安装证书机器人:

```
sudo apt install certbot
```

然后生成密钥:

```
sudo certbot certonly --standalone -d email.example.com
```

配置 Postfix 以使用这些键。在/etc/postfix/main.cf 中添加或编辑以下参数:

```
smtpd_tls_cert_file = /etc/letsencrypt/live/email.example.com/fullchain.pem
smtpd_tls_key_file = /etc/letsencrypt/live/email.example.com/privkey.pem
smtpd_tls_security_level=may
smtp_tls_CApath=/etc/letsencrypt/live/email.example.com
smtp_tls_security_level=may
```

然后重新启动 Postfix:

```
sudo service postfix restart
```

# 试验

发送包含以下命令的测试电子邮件:

```
echo "This is the body of the email" | mail -r test@example.com -s "This is the subject" your_email@gmail.com
```

通过在线服务检查垃圾邮件分数，例如:【https://www.mail-tester.com/】T2(不是广告)。

*原载于*[*https://bogomolov . tech*](https://bogomolov.tech/postfix-self-hosted-smtp/)*。*