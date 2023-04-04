# 创建安全的 Android 应用程序。

> 原文：<https://blog.devgenius.io/creating-secure-android-applications-d41531dbedd5?source=collection_archive---------5----------------------->

![](img/9e8febea8d3a9d908faa32bdba36381c.png)

安全性是每个应用程序的一个重要方面。极其重要的是，我们要采取措施来帮助我们创建安全的应用程序。由于 Android 是一个开源操作系统，它对我们来说是一个巨大的挑战，以确保我们的应用程序的完整性。您采取了一些措施来提高应用程序的安全性。让我们看看我们可以确保它的一些方法。

**代码混淆**

第一件重要的事情是启用代码混淆。这可以通过在应用程序的构建 gradle 文件中将 *minifyenabled* 选项变量设置为 true 来实现。

在 Android studio 版以后，模糊处理是通过 R8 完成的，它取代了 pro-guard 并自动启用。R8 规范使得人们很难仅仅通过直接反编译来逆向工程你的应用程序。这个过程用一些其他字母替换你的类和变量的名字，这使得任何人都很难直接阅读和理解代码。

**Android Root 检测和设备完整性测试**

根设备是那些拥有自定义操作系统的 android 手机，允许用户在手机上拥有额外的权限，如查看系统文件，覆盖或更改它们。使用这样的设备，任何恶意方都可以试图找到您的应用程序源代码和存储文件，如共享的首选项和数据库。为了防止您的应用程序在根设备上运行，您可以实施一系列检查，告诉我们设备的安全性是否受到威胁。

这样的解决方案有，检查 SU 二进制文件，BusyBox 二进制文件，检查 Magisk 包，等等。有许多库可以帮助我们进行根检测，其中一个是 [RootBeer](https://github.com/scottyab/rootbeer) ，它本身为我们实现了所有这些检查，并给我们一个或真或假的结果，请确保记住假阳性结果。

```
// In your Gradle.build file add the following line
implementation 'com.scottyab:rootbeer-lib:0.0.7'//In your Main Activity or the First Launched View Perform the check.RootBeer rootBeer = new RootBeer(context);
if (rootBeer.isRooted()) {
    //we found indication of root
} else {
    //we didn't find indication of root
}
```

另一个帮助我们检测移动设备整体完整性的解决方案是谷歌的安全网认证 API，它以 JWT 的形式给出回应，告诉我们 Android 设备的完整性。这是谷歌的一个演示项目，其中包含示例[实现](https://github.com/googlesamples/android-play-safetynet/)。

**加密和安全的 Web 服务器通信。**

几乎我们所有的应用程序都使用 web API 请求与 web 服务器通信。确保客户端和服务器之间的安全通信是我们可以采取的下一步，这可以通过加密客户端服务器通信和证书锁定来实现。例如，如果您有一个 GET 类型的请求，您应该加密请求参数，服务器应该首先解密请求。同理。如果您有 post 类型的请求呼叫。你应该加密它的主体。请求的加密应该是唯一的，应该有一个唯一的密钥与之关联。

下面是这个用例的一个 [**实现**](https://gist.github.com/humayuntanwar/b3c0512018df24fb667d70b2bbb513b5) ，它使用改造库作为网络层。

我们可以实现 web 服务安全性的另一种方法是使用[网络安全配置](https://developer.android.com/training/articles/security-config)，网络安全配置功能使用一个 XML 文件，您可以在其中指定应用程序的设置。您必须在应用的清单中包含一个指向此文件的条目。。

**安全本地存储**

本地存储为我们提供了一个很好的机制来存储数据并提供流畅的用户体验。本地数据可以存储在文件、共享首选项或数据库中。但是如果用户使用任何恶意技术或渗透测试工具，所有这些都可以被用户访问。对我们来说，确保将数据以加密形式存储在本地存储中是极其重要和必要的。有许多不同的方法可以实现这一点。如果您使用 sqlite db 作为本地存储，我们可以使用 [SQLCipher](https://github.com/sqlcipher/sqlcipher) 库来加密我们的数据库，如果您使用 android 组件的一部分 room db，我们可以使用[CWAC-安全室](https://github.com/commonsguy/cwac-saferoom)库来加密我们的数据库。对于共享首选项，您可以使用简单的加密和解密功能来存储数据，或者尝试以下提供加密共享首选项完整解决方案的库之一， [secure-preferences](https://github.com/scottyab/secure-preferences) ， [ksc-pref](https://github.com/cioccarellia/ksc-pref) 。

**调试模式和应用程序日志**

有时，当我们急于发布我们的应用程序时，我们会忘记勾选所有正确的框，这可能会导致 APK 文件很容易被逆向工程和调试，这可能会导致敏感信息和代码泄漏。在发布 APK 或软件包之前，请确保设置 deguggable = false。几乎所有时间我们都在使用 log.d 或 log.v 来帮助我们开发应用程序。当我在应用程序上工作时，我使用日志来验证我的请求和响应，这样的敏感信息如果在错误的人手中会导致整个系统的失败，我们必须确保我们在开发阶段删除或禁用所有这样的日志。

密钥、API URLs 和常量等敏感数据需要以安全的格式存储在属性文件中或以加密格式存储。另一个好的实践是在运行时从服务器接收这样的信息，以响应请求。

**一些应用特定安全特征如下:**

**剪贴板复制粘贴和截图预防。**

剪切板是 android 中一个强大的工具，它可以用来执行攻击，例如直接将 sql 注入到我们的应用程序中，或者允许用户从应用程序中复制敏感信息。为了防止用户复制和粘贴，你可以做以下事情。[防止在编辑文本上复制粘贴。](https://stackoverflow.com/a/12331404)

为了防止用户截屏，您可以在活动中添加以下代码

```
getWindow().setFlags(WindowManager.LayoutParams.FLAG_SECURE,
                           WindowManager.LayoutParams.FLAG_SECURE);
```

**一次性用户验证**

另一种增加额外安全层的方法是使用一次性密码作为登录验证系统，这可以使用一个 signal 或 Firebase 或任何其他服务来实现。

这些是您可以采取的一些措施，以确保您的应用程序的完整性，还有更多方法可能是特定于您的用例的。在发布你的应用程序之前，一定要记住把它们勾掉。快乐编码。