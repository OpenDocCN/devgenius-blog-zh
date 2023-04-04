# 如何在一个 Android 项目中检查互联网连接？

> 原文：<https://blog.devgenius.io/how-to-check-the-internet-connection-in-an-android-project-4371aafa59ca?source=collection_archive---------13----------------------->

首先，您需要在文件的顶部导入`ConnectivityManager`和`NetworkInfo`类:

```
import android.net.ConnectivityManager;
import android.net.NetworkInfo;
```

接下来，您需要通过调用`getSystemService()`并传入`CONNECTIVITY_SERVICE`常量来获取对`ConnectivityManager`的引用。这将允许您访问有关设备网络连接的信息。

```
ConnectivityManager cm = (ConnectivityManager)getSystemService(Context.CONNECTIVITY_SERVICE);
```

然后，您可以使用`ConnectivityManager`的`getActiveNetworkInfo()`方法来获取一个`NetworkInfo`对象，该对象表示当前活动的网络连接，如果存在的话。

```
NetworkInfo activeNetwork = cm.getActiveNetworkInfo();
```

然后，您可以通过检查`NetworkInfo`对象的`isConnectedOrConnecting()`方法来检查设备是否连接到互联网。如果设备连接到互联网，该方法返回`true`，否则返回`false`。

```
boolean isConnected = activeNetwork != null && activeNetwork.isConnectedOrConnecting();
```

最后，您可以使用一个`if`语句来检查`isConnected`的值，并根据连接状态采取适当的措施。

```
if (isConnected) {
    // do something
} else {
    // show an error message or do something else
}
```

您还需要向 AndroidManifest.xml 文件添加`ACCESS_NETWORK_STATE`权限，以允许您的应用程序访问有关设备网络连接的信息:

```
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

谢谢大家！