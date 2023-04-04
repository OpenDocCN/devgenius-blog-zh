# 容易设置查尔斯代理拦截 Android 模拟器的 HTTPS 流量

> 原文：<https://blog.devgenius.io/setup-charles-proxy-to-intercept-https-traffic-of-android-emulator-591baba5d7ec?source=collection_archive---------0----------------------->

对于 Web 或应用程序开发，通常需要调试和查看应用程序的 HTTPS 流量。为了拦截流量，您必须使用中间人技术在您的应用程序和 API 端点之间进行解密和加密。

让我们用 Android 模拟器详细演练一下…

# A.设置 Android 模拟器(在 Android Studio 中)

您必须使用模拟器系统映像，而不使用播放存储

![](img/e4efcb2f1eb36ade26772ff3a82bcb25.png)

选择没有播放商店图标的图像

![](img/b219e2f51c5d04319f5c767e63dc720c.png)

使用 Android Q

![](img/4430105fb1158f72d82f4a5fb27e949d.png)

(可选)显示高级设置→设置为“冷启动”

> 存储模拟器的新文件夹将在`%USERPROFILE%\.android\avd`创建

# B.导出根证书

保存查尔斯根证书

![](img/57807e0675e3515c8d7c26c3b11c937d.png)

将这个“pem”文件重命名为 android 特定格式的证书文件。

> 你可以在 **Git Bash** 中使用 openssl

```
openssl x509 -inform PEM -subject_hash_old -in charles.pem | head -1
```

> 名称格式为" <hash-value>" + ".0"
> (只需将" . 0 "作为一个字符串与哈希值字符串连接起来)</hash-value>

![](img/5437b6b5827a9019a652b5c6b8855c84.png)

# C.启动模拟器

> Windows
> 中的仿真器路径`%LOCALAPPDATA%\Android\Sdk\emulator`

```
cd %LOCALAPPDATA%\Android\Sdk\emulator emulator -avd Pixel_3a_XL_API_29 -writable-system -no-snapshot-load
```

![](img/c8bd8a600db508bfbee8667e3b4be74a.png)

# D.根证书和推送根证书

> Windows 中的亚行路径
> `%LOCALAPPDATA%\Android\Sdk\platform-tools`

用`adb`执行命令

*   适用于 Android Q 及以上版本(API 等级≥ 29)

```
adb root 
adb shell avbctl disable-verification 
adb reboot 
```

![](img/8ebf1659ad3639c086ecd2d41259b597.png)

```
adb root 
adb remount  
adb push 4a40cad5.0 /system/etc/security/cacerts/ 
adb -e shell chmod 644 /system/etc/security/cacerts/4a40cad5.0 
adb reboot
```

![](img/ff5cab71c1a0a093b6ad994ab91777e7.png)

*   对于较旧的 Android (API 级别< 29)

```
adb root 
adb remount 
adb push 4a40cad5.0 /system/etc/security/cacerts/ 
adb -e shell chmod 644 /system/etc/security/cacerts/4a40cad5.0 
adb reboot
```

# E. Verify Trusted Cert & Setting APN

1.  Settings → Security → Encryption & Credentials → Trusted credentials

![](img/76f757775cd545c37f374d3beb2b65dc.png)

2\. Disable Wi-Fi

3\. Network & Internet → Mobile network → Advanced → Access Point Names

> *集合名称，APN 具有任意值，代理&端口应该跟随查尔斯代理*

![](img/a4628792f8e6e85879111941b78a3e33.png)

4.选择 APN

![](img/5fbf473ecd3f646a8c1175848ef6a09e.png)

5.确认

![](img/0813d76a60107d0267b4d9e905de7b61.png)

如果代理配置不匹配，或者 Charles 没有启动

![](img/39e998d90cb701bcf83856c7dfcafa51.png)

如果一切正常

一旦你完成了上述所有步骤，你应该能够看到所有的加密流量从你的 Android 模拟器发送！