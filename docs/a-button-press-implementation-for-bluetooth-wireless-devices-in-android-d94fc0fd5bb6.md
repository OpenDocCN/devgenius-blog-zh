# Android 中蓝牙设备的按键实现

> 原文：<https://blog.devgenius.io/a-button-press-implementation-for-bluetooth-wireless-devices-in-android-d94fc0fd5bb6?source=collection_archive---------5----------------------->

大家好！

这是我第一篇关于 Android 和整个媒体页面开发的文章。

# 简短的介绍

在 Android 项目中实现蓝牙的过程中，我花了很长时间和精力，在这个领域做了一些研究:如何在不使用**广播接收器**中的这种`Intent.ACTION_MEDIA_BUTTON`的情况下更好地实现一个“按钮按压”，这在我们的时代已经不再实际。

因为…

引自谷歌:

作为 Android 8.0 (API 级别 26)后台执行限制的一部分，针对 API 级别 26 或更高的应用程序不能再在其清单中注册隐式广播的广播接收器

![](img/d0c22baa3555bfd5baeacf34f3f5c7f0.png)

好吧。那么在这种情况下我们该怎么做呢？

我尝试创建了一个单独的服务，在后台自己创建的服务中拦截一个活动的**媒体按钮**的按下。我在下一个例子中的做法是:

```
class BluetoothService : Service {override fun onBind(intent : Intent) : IBinder {
     //Return the communication channel to the service.
}override fun onCreate() {
     super.onCreate()
}@Override
override fun onStartCommand(intent : Intent, flags : Int, startId : Int) : Int{
     ...
     return super.onStartCommand(intent, flags, startId);
} override fun onDestroy() {
     super.onDestroy()
 }
}
```

因此，通过创建一个基本服务(当然是在 Android 8 之前),一切看起来都没问题，应该可以正常工作。在**onstart 命令**中。我们可以简单地实现本地类，比如 BluetoothAdapter、BluetoothProfile 等。

但是…我们不能再以服务的方式这样做了。

![](img/387330a5380f588fe6b4bc071957f8e3.png)

# 通往巅峰的路还很长…

花这么多时间在这上面，再加上谷歌最新最棒的更新，后台工作应该受到限制…我知道作为一个普通的开发人员，我试图使用一些技巧？让这项服务恢复正常。我不知道谷歌会不会在我的文章后把它删掉，但似乎只对 Android 中的 BT 功能没问题，所以我很怀疑。谁知道呢..

## 首先。

我们有一个很好的监听器，叫做 ServiceListener，它位于一个本地 BluetoothProfile 类中，允许我们监听和处理 BT 的连接状态:

```
bluetoothProfileListener = object : BluetoothProfile.ServiceListener(){fun onServiceConnected(profile : Int, proxy: BluetoothProfile){}fun onServiceDisconnected(profile : Int){}}
```

我们需要创建一个 MediaSession 回调监听器来拦截我们在蓝牙设备上的按钮点击:

```
private val mediaSessionCallback: MediaSession.Callback = object :        MediaSession.Callback() {        
override fun onMediaButtonEvent(mediaButtonIntent: Intent): Boolean {            

val mediaButtonAction = mediaButtonIntent.action   

if (Intent.ACTION_MEDIA_BUTTON == intentAction) {                
val ev = mediaButtonAction.getParcelableExtra<KeyEvent>(Intent.EXTRA_KEY_EVENT)                
if (event != null) {                    
     val action = event.action                    
          if (action == KeyEvent.ACTION_DOWN) {                                                                   
          }                
     }            
}            
     return super.onMediaButtonEvent(mediaButtonIntent)        
     }    
}
```

..我们将用它来设置 onServiceConnected 中的 **AudioSession 的回调。有趣是吗？我通过简单地尝试使用我所拥有的东西找到了这种方法，而不喜欢基于前台的服务功能。**

```
val audioSession = MediaSession(context, "BLUETOOTH")                    mBluetoothHeadset = proxy as BluetoothHeadset                                     audioSession.isActive = true                    audioSession.setCallback(mCallback)
```

正如我所说的，我们将在 ServiceConnected 的 override 方法中使用这段代码，这是我们几行之前在这里创建的。

不要忘记在蓝牙完成工作后禁用它&同时实现其他对象，这些在 developers.android.com 教程中有介绍

# 摘要

在谷歌 Android 开发者页面中，缺少如何正确使用这个东西的信息，也缺少初始化 MediaSession 的信息。回调在那里显示为一个外部对象。出于某些原因，这种实现可能不好，但至少它可以作为临时解决方案的一个小而有用的工具。

为我第一篇文章中的一些错误道歉。当然，你可以用你的建议来评论如何用另一种方式来做，或者我的建议如何升级。

感谢您的关注。

干杯！

#科特林