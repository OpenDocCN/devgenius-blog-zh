# 在生产 android 应用上启用关键日志/功能进行调试

> 原文：<https://blog.devgenius.io/enable-critical-logs-features-on-production-android-apps-for-debugging-af4de0133ba6?source=collection_archive---------1----------------------->

![](img/ff87fb8f23a331fd3c3c702ce2a074e2.png)

Pawel Czerwinski 在 [Unsplash](https://unsplash.com/s/photos/app-logs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

正如我们所知，当我们捆绑 android 应用程序进行发布时，某些日志(调试日志)会被删除，在应用程序用于生产之前禁用整个应用程序日志是一种很好的做法。此外，一些有助于调试的特性也将被开发人员使用一些编译时标志禁用，如

> BuildConfig 调试

很多时候，我们希望调试生产应用程序来重现某些场景，并通过 logcat 或上传信息到 web 服务器来捕获关键信息。

> **这就是安卓系统属性派上用场的地方。一般来说，系统属性不能由应用程序来设置，但是这些被 android 框架和一些系统应用程序用作全局系统属性来简化某些事情。然而，有两个属性可以被运行在开发机器上的 adb 守护进程修改。**

> 调试
> 
> 日志.标签

> 例如:adb shell setprop debug.myapp true
> 
> 例如:ADB shell set prop log . tag . myapp . debug true

在应用程序方面，我们的关键日志语句或代码块可以使用 system 属性来保护。

> 例如:if(utils . get system property(" debug . myapp "){
> 
> //这里是关键代码
> 
> }

//Utils.kt

```
**fun** getSystemProp(key: String, default: String? = **null**): String? {
    **try** {
        **val** c = Class.forName(**"android.os.SystemProperties"**)

        **val** method = c.getDeclaredMethod(**"get"**, String::**class**.*java*)

        **val** value = method.invoke(**null**, key) **as** String

        **return if** (value.*isBlank*()) {
            default
        } **else** {
            value
        }
    } **catch** (e: Exception) {
        e.printStackTrace()
    }

    **return** default
}
```

最终想法:

默认情况下，设备上没有任何这些自定义系统属性。但是当我们使用 adb shell 设置属性时，它将被创建。

我们可以重新启动应用程序来启用关键代码块，这将有助于调试。完成调试后，只需重置属性即可。无论如何，设备重新启动后，这些属性将不会在设备上看到，这意味着它们是临时的，直到重新启动。

这对于调试生产应用程序、自动化测试生产应用程序或启用/禁用应用程序某些功能的预览非常有用。然而，我在这里提供了许多调试选项中的一个，但并不推荐使用它。使用之前，请仔细分析与隐私相关的问题。可能不是将属性名定义为常量，而是可以从后端提取或编码它。