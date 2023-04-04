# RHEL/CentOS 7 修复让我们加密变化

> 原文：<https://blog.devgenius.io/rhel-centos-7-fix-for-lets-encrypt-change-8af2de587fe4?source=collection_archive---------0----------------------->

## [第 0 根安全网络](https://www.0snet.com)

## 在**2021 年 9 月 30 日**之前采取行动以避免问题

![](img/88eb6cd3d0274061a3036fba4eddf508.png)

由[凯文·霍尔瓦特](https://unsplash.com/@hidd3n?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**TL；DR** — *对于 Let's Encrypt 颁发的 TLS 证书，默认链中的根证书(DST Root CA X3)于 2021 年 9 月 30 日***到期。由于他们独特的方法，过期的证书将继续作为证书链的一部分，直到 2024 年。此* [*影响 RHEL/CentOS 7 服务器*](https://medium.com/@0snet/lets-encrypt-change-affects-openssl-1-0-x-and-centos-7-49bd66016af3) *上的 OpenSSL 1.0.2k，并将导致应用程序/工具无法建立 TLS/HTTPS 连接，并显示* ***证书已过期*** *消息。**

**从 24/9/21 开始，* *升级 ca-certificates 包(*[*2021 . 2 . 50–72*](https://access.redhat.com/errata/RHBA-2021:3649)*)应该可以修复该问题。* [*版本 2021 . 2 . 50–72 删除 DST 根 CA X3*](https://access.redhat.com/articles/6338021) *。**

**截至 2011 年 17 月 9 日，唯一可用的解决方案是将根证书列入黑名单，如下所示**

```
*trust dump --filter "pkcs11:id=%c4%a7%b1%a4%7b%2c%71%fa%db%e1%4b%90%75%ff%c4%15%60%85%89%10" | openssl x509 | sudo tee /etc/pki/ca-trust/source/blacklist/DST-Root-CA-X3.pem sudo update-ca-trust extract*
```

**此解决方案类似于* [*Sectigo 根和中间证书到期的替代修复方案——2020 年 5 月*](https://access.redhat.com/articles/5117881) *。**

*L **onger 版本** — [Let's Encrypt 决定在他们的默认证书链中使用一个特殊的交叉签名](https://letsencrypt.org/2020/12/21/extending-android-compatibility.html)来扩展对旧 Android 设备的支持。然而，由于默认情况下 **OpenSSL 1.0.2** 构建和验证证书路径的方式，路径中存在过期的根证书会导致证书验证失败。*

***OpenSSL 1.1.x** 和更新的版本不受影响，因为它们可以构建到不同根(ISRG 根 X1)的更短的证书路径，让我们成功地加密证书和验证链。因此，这个问题对于老的但受支持的平台来说非常具体，比如 **RHEL 7** 和 **Ubuntu 16.04** 。他们的根存储中有一个受信任的备用根(ISRG 根 X1 ),但是 OpenSSL 1.0.2 并没有首先选择它。*

# *潜在问题*

***我们来加密**是全球发行规模最大的 CA。因此，当服务器无法验证它们的证书时，问题会以多种形式出现。其中最简单的是 *certbot* 本身可能会失败，因为它无法访问 API 来获得免费的加密证书，*

```
***$ sudo faketime '1 Oct 2021' certbot renew --dry-run**
Saving debug log to /var/log/letsencrypt/letsencrypt.log
[...]
Revocation status for /etc/letsencrypt/archive/0snet/cert22.pem is unknown
Cert not due for renewal, but simulating renewal for dry run
Plugins selected: Authenticator webroot, Installer None
Starting new HTTPS connection (1): acme-staging-v02.api.letsencrypt.org
Failed to renew certificate 0snet with **error: __str__ returned non-string (type Error)** [...]
All simulated **renewals failed**. The following certificates could not be renewed:
  /etc/letsencrypt/live/0snet/fullchain.pem (failure)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
**1 renew failure(s)**, 0 parse failure(s)*
```

*上面的错误没有告诉您太多，但是当您尝试手动调用 API 时，它会变得很明显，*

```
***$ faketime '1 Oct 2021' wget** [**https://acme-staging-v02.api.letsencrypt.org/directory**](https://acme-staging-v02.api.letsencrypt.org/directory) **-O-**
--2021-10-01 00:00:00--  [https://acme-staging-v02.api.letsencrypt.org/directory](https://acme-staging-v02.api.letsencrypt.org/directory)
Resolving acme-staging-v02.api.letsencrypt.org (acme-staging-v02.api.letsencrypt.org)... 172.65.46.172, 2606:4700:60:0:f41b:d4fe:4325:6026
Connecting to acme-staging-v02.api.letsencrypt.org (acme-staging-v02.api.letsencrypt.org)|172.65.46.172|:443... connected.
**ERROR: cannot verify acme-staging-v02.api.letsencrypt.org's certificate, issued by ‘/C=US/O=Let's Encrypt/CN=R3’:
  Issued certificate has expired.**
To connect to acme-staging-v02.api.letsencrypt.org insecurely, use `--no-check-certificate'.*
```

*在上面的命令中， ***wget*** 是有意使用的，因为它为 **HTTPS** 使用了 **OpenSSL** 库，而不像*使用了 **NSS** ( [Mozilla 网络安全服务](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/Overview))库并且不受影响。**

# **可能的修复**

**这个问题可以通过发布新版本的 **OpenSSL 1.0.2** 来解决，默认启用标志**X509 _ V _ FLAG _ TRUSTED _ FIRST**。这是针对 [**Ubuntu 16.04**](https://bugs.launchpad.net/ubuntu/+source/openssl/+bug/1928989) 采取的做法。但是，更改 OpenSSL 的默认标志可能会导致应用程序的行为发生变化。在长期稳定的版本中，通常会避免这种情况。因此，我不确定 RHEL 7 号是否会采用这种方式。**

**另一个解决方案是发布一个新版本的***CA-certificates***包，它不包括根存储中的 **DST 根 CA X3** 。这将工作，因为 **ISRG 根 X1** 已经存在，以验证让我们加密证书。然而， *ca-certificates* 包是来自 Mozilla 根存储的一组未经修改的 ca 证书。因此， **DST 根 CA X3** 需要从 [**NSS**](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/NSS_3.66_release_notes) 中删除，这可能需要时间。**

****24/9/21**更新——*新版****ca-certificates****包(*[*2021 . 2 . 50–72*](https://access.redhat.com/errata/RHBA-2021:3649)*)已经发布。它从根存储中删除* ***DST 根 CA X3*** *。不再需要下面的手动步骤。***

**如果不更新 OpenSSL(or)*CA-certificates*包，唯一的解决方案就是从根存储中删除 **DST 根 CA X3** 。这可以通过将其添加到黑名单( *man update-ca-trust* )来干净地完成。**

> **在黑名单子目录/usr/share/PKI/ca-trust-source/black list/or**/etc/PKI/ca-trust/source/black list/**中，您可以安装一个或多个 DER 文件格式或 PEM(开始/结束证书)文件格式的证书。出于各种目的，每个证书都将被视为不可信。**

## **支持**

```
**cp -i /etc/pki/tls/certs/ca-bundle.crt ~/ca-bundle.crt-backup**
```

## **将证书添加到黑名单目录**

```
**trust dump --filter "pkcs11:id=%c4%a7%b1%a4%7b%2c%71%fa%db%e1%4b%90%75%ff%c4%15%60%85%89%10" | openssl x509 | sudo tee /etc/pki/ca-trust/source/blacklist/DST-Root-CA-X3.pem**
```

## **更新根存储**

```
**sudo update-ca-trust extract**
```

## **验证移除**

```
**diff ~/ca-bundle.crt-backup /etc/pki/tls/certs/ca-bundle.crt**
```

## **抽样输出**

```
**$ diff ~/ca-bundle.crt-backup /etc/pki/tls/certs/ca-bundle.crt
860,881d859
< # DST Root CA X3
< -----BEGIN CERTIFICATE-----
< MIIDSjCCAjKgAwIBAgIQRK+wgNajJ7qJMDmGLvhAazANBgkqhkiG9w0BAQUFADA/
< MSQwIgYDVQQKExtEaWdpdGFsIFNpZ25hdHVyZSBUcnVzdCBDby4xFzAVBgNVBAMT
< DkRTVCBSb290IENBIFgzMB4XDTAwMDkzMDIxMTIxOVoXDTIxMDkzMDE0MDExNVow
< PzEkMCIGA1UEChMbRGlnaXRhbCBTaWduYXR1cmUgVHJ1c3QgQ28uMRcwFQYDVQQD
< Ew5EU1QgUm9vdCBDQSBYMzCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEB
< AN+v6ZdQCINXtMxiZfaQguzH0yxrMMpb7NnDfcdAwRgUi+DoM3ZJKuM/IUmTrE4O
< rz5Iy2Xu/NMhD2XSKtkyj4zl93ewEnu1lcCJo6m67XMuegwGMoOifooUMM0RoOEq
< OLl5CjH9UL2AZd+3UWODyOKIYepLYYHsUmu5ouJLGiifSKOeDNoJjj4XLh7dIN9b
< xiqKqy69cK3FCxolkHRyxXtqqzTWMIn/5WgTe1QLyNau7Fqckh49ZLOMxt+/yUFw
< 7BZy1SbsOFU5Q9D8/RhcQPGX69Wam40dutolucbY38EVAjqr2m7xPi71XAicPNaD
< aeQQmxkqtilX4+U9m5/wAl0CAwEAAaNCMEAwDwYDVR0TAQH/BAUwAwEB/zAOBgNV
< HQ8BAf8EBAMCAQYwHQYDVR0OBBYEFMSnsaR7LHH62+FLkHX/xBVghYkQMA0GCSqG
< SIb3DQEBBQUAA4IBAQCjGiybFwBcqR7uKGY3Or+Dxz9LwwmglSBd49lZRNI+DT69
< ikugdB/OEIKcdBodfpga3csTS7MgROSR6cz8faXbauX+5v3gTt23ADq1cEmv8uXr
< AvHRAosZy5Q6XkjEGB5YGV8eAlrwDPGxrancWYaLbumR9YbK+rlmM6pZW87ipxZz
< R8srzJmwN0jP41ZL9c8PDHIyh8bwRLtTcm1D9SZImlJnt1ir/md2cXjbDaJWFBM5
< JDGFoqgCWjBH4d1QB7wCCZAA62RjYJsWvIjJEubSfZGL+T0yjWW06XyxV3bqxbYo
< Ob8VZRzI9neWagqNdwvYkQsEjgfbKbYK7p2CNTUQ
< -----END CERTIFICATE-----
<**
```

# **何时打补丁**

**由 **DST 根 CA X3** 颁发的唯一有效证书是交叉签名根证书 [**IdenTrust 商业根 CA 1** 和 **ISRG 根证书 X1**](https://crt.sh/?Identity=%25&iCAID=276&exclude=expired) 。这些已经是 RHEL/CentOS 7 服务器上根存储的一部分。**

```
**$ trust list | grep -C2 "IdenTrust Commercial Root CA 1"
pkcs11:id=%ed%44%19%c0%d3%f0%06%8b%ee%a4%7b%be%42%e7%26%54%c8%8e%36%76;type=cert
    type: certificate
    label: IdenTrust Commercial Root CA 1
    trust: anchor
    category: authority$ trust list | grep -C2 "ISRG Root X1"
pkcs11:id=%79%b4%59%e6%7b%b6%e5%e4%01%73%80%08%88%c8%1a%58%f6%e9%9b%6e;type=cert
    type: certificate
    label: ISRG Root X1
    trust: anchor
    category: authority**
```

**因此，即使在 2021 年 9 月 30 日**之前**，从根存储中移除 **DST 根 CA X3** 也是安全的。**

# **最后的想法**

**让我们加密[是否提供了一个选项](https://medium.com/@0snet/lets-encrypt-change-affects-openssl-1-0-x-and-centos-7-49bd66016af3#bd4c)来自动获取新的证书作为替代链的一部分，即没有过期的根证书。然而，考虑到 2 亿个活跃的加密证书，期望它们都使用备用链是不切实际的。**

**不幸的是，唯一的解决方案是修补我们的 RHEL/CentOS 7 服务器。我希望这是一个简单的系统更新，但它不是。希望几个月后我们会有更好的解决方案。**

# **参考**

1.  ****Ubuntu** ，[即将到期的信任锚兼容性问题](https://bugs.launchpad.net/ubuntu/+source/openssl/+bug/1928989)**
2.  ****NSS** ，[发行说明 3.66](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/NSS_3.66_release_notes)**
3.  ****RHEL** ， [Sectigo 根和中级证书有效期—2020 年 5 月](https://access.redhat.com/articles/5117881)**

***在第 0 根，我们提供解决方案* [*第 0 根安全网络— 0SNet*](https://www.0snet.com/) *使用 TLS 客户端证书保护组织的内部 web 应用。请检查我们的产品，它易于部署，并可作为图像显示在*[*AWS*](https://0snet.info/#install.aws)*，*[*GCP*](https://0snet.info/#install.gcp)*和*[*Azure*](https://0snet.info/#install.azu)*上。***