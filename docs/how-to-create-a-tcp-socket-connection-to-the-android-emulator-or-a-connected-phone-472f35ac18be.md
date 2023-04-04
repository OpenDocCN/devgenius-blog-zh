# 如何创建到 Android 模拟器或连接的手机的 TCP 套接字连接

> 原文：<https://blog.devgenius.io/how-to-create-a-tcp-socket-connection-to-the-android-emulator-or-a-connected-phone-472f35ac18be?source=collection_archive---------0----------------------->

## 在手机和电脑之间传输数据流

![](img/e345dd115a45a9655cdcb6bc6bbd7b35.png)

照片由来自[佩克斯](https://www.pexels.com/photo/person-holding-smartphone-while-using-laptop-1181244/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的[克里斯蒂娜·莫里洛](https://www.pexels.com/@divinetechygirl?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

我已经用我的手机作为网络摄像头有一段时间了，但当我想创建自己的应用程序，将相机信号从我的手机传输到我的电脑时，我无法找到任何好的资源来解释如何做到这一点。所以在花了太多时间试图弄明白之后，我想创建一个简短的教程。

在手机和电脑之间传输数据有很多原因。除了前面提到的摄像头信号，你还可以从你的手机上获取 GPS 数据，使用它的加速度传感器，甚至在它上面显示额外的信息作为第二个屏幕。

似乎在 Android 设备上运行服务器，在您的计算机上运行客户端是更容易的选择。我第一次在 Android 的客户端上试了一下，它在模拟器上运行得很好。然而，当我在用 USB 线连接的手机上测试它时，它再也无法连接了。我还没能弄清楚为什么，但是对于两个设备之间的连接来说，哪个是服务器并不重要。因此，让我们看看如何在您的计算机和 Android 模拟器或您的手机之间创建 TCP 套接字连接。

# 创建服务器

让我们从在 Android 端创建一个非常简单的服务器应用开始。打开 [Android Studio](https://developer.android.com/studio) 并创建一个包含空活动的新项目。然后为服务器添加一个新的 Java 类。

Android 使用普通的 Java [ServerSockets](https://developer.android.com/reference/java/net/ServerSocket) ，所以我们可以非常容易地创建一个基本的服务器。唯一值得一提的是，我们必须在一个单独的线程中启动套接字，因为 Android 不允许主线程上有任何网络代码，如果你尝试，它会抛出一个异常。

在应用程序的主活动中，我们只需要创建一个新的服务器对象。

最后，我们需要给我们的应用程序 internet 权限，这可以通过将这些行添加到 AndroidManifest.xml 文件中来完成。

```
<uses-permission android:name="android.permission.INTERNET" >
</uses-permission>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" >
</uses-permission>
```

# 简单的客户

我们还需要一个简单的 TCP 客户端来测试我们是否可以连接到我们的服务器。我正在使用我自己的 C++网络库，但是为了本教程，让我们用 Java 创建一个简单的客户机:

然而，如果你试图连接到运行在模拟器上的服务器或者一个连接的电话，它现在还不能工作。您可能在这一行`this.socket = new Socket( “127.0.0.1” , 12345 );`中注意到我们正在连接到 localhost，考虑到我们想要连接到不同的设备，这有点奇怪。但这是有原因的:

# 亚洲开发银行(Asian Development Bank)

[**ADB** 或 **Android 调试桥**](https://developer.android.com/studio/command-line/adb) 是 Android 开发的官方工具。它用于与仿真器和设备通信，这正是我们想要做的。首先，我们需要下载 [ADB/SDK 平台工具](https://developer.android.com/studio/releases/platform-tools)并将其解压到某个地方。然后，我们可以在提取的文件夹中打开一个终端(Windows 提示:在浏览器的导航行中单击并键入" *cmd"* ，点击 ENTER ，它将在当前文件夹中打开一个命令行界面)。在那里，我们需要输入以下命令:

`adb forward tcp:12345 tcp:12345`

这将把 TCP 流量从计算机本地主机的端口 12345 转发到同一端口上的设备。在此之后，运行在您的计算机上的客户端应该能够连接到运行在您的 Android 设备上的服务器。

# 重要说明

每次在模拟器和 USB 连接的智能手机之间切换时，您都需要再次输入 adb 命令。

另一种方法是在连接前直接从客户端程序调用命令:

`C++: system( “*PATH_TO*/adb.exe forward tcp:12345 tcp:12345” );`

`Java: Runtime.getRuntime().exec( “*PATH_TO*/adb.exe forward tcp:12345 tcp:12345” );`

一旦你知道如何做，用 TCP 套接字将 Android 模拟器或连接的智能手机连接到你的电脑并不太难。我花了一段时间才弄明白，但希望这篇教程能帮助你更快地使用它。