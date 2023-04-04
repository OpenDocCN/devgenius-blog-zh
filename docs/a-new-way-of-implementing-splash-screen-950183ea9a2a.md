# 实现闪屏的新方法！。

> 原文：<https://blog.devgenius.io/a-new-way-of-implementing-splash-screen-950183ea9a2a?source=collection_archive---------12----------------------->

安卓|科特林

![](img/c5bd9086786b8d7978ae52d5b2d3dcfc.png)

嘿伙计们！

这是 android 想出的一个非常简单的功能，它将改变这些年来使用闪屏的方式，成为最优雅的方式。将传统的包含布局设计和处理程序的 Splash 活动缩小到一个处理所有事情的主题中，这是一件令人惊奇的事情。

我在 Youtube 上传了一段视频。请看一看！..

## **听起来很酷，对吧，让我们不要等待，投入到开发中。**

## 1.首先，设置主题

让我们从创建闪屏的主题开始。我将这个主题命名为 SplashTheme，它包含了一组属性，让我们来浏览一下

**I)windowSplashScreenBackground:-**这个属性设置启动画面的背景颜色。

**ii)windowSplashScreenAnimatedIcon:-**这基本上是应用程序图标，是可以在活动中制作动画的品牌详细信息

**iii)windowsplasscreenanimationduration:-**这是屏幕在屏幕上可见的持续时间，移动到下一个主题并显示应用程序中的第一个活动。建议不要超过 1000 毫秒。

**iv)post splash screen theme:-**通过在该属性中添加主主题，它将 splash 主题替换为用于活动的主 appcompact 主题。

**v)windowSplashScreenBrandingImage:-**这是一个属性，我们可以在闪屏上添加品牌，它将显示在闪屏的底部。该功能只能在 android 12 及更新版本上使用。

```
<style name="SplashTheme" parent="Theme.SplashScreen">
    <item name="windowSplashScreenBackground">@color/purple_200</item>
    <item name="windowSplashScreenAnimatedIcon">@drawable/splash_icon</item>
    <item name="windowSplashScreenAnimationDuration">500</item>
    <item name="android:windowSplashScreenBrandingImage" tools:targetApi="s">@drawable/splash_icon</item>
    <item name="postSplashScreenTheme">@style/Theme.NewSplashScreen</item>
</style>
```

## 2.在活动中添加闪屏

为了将闪屏添加到活动中，android 有一个名为 installSplashScreen()的函数。这个函数使用主题的属性来显示活动的闪屏。

```
override fun onCreate(savedInstanceState: Bundle?) {
    *installSplashScreen*()
}
```

## 3.在清单上添加闪屏

```
<activity
    android:name=".MainActivity"
    android:exported="true"
    android:theme="@style/SplashTheme">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

我们需要在启动器活动的清单文件中添加新的闪屏主题。稍后，当这个主题启动时，一旦 splash 持续时间结束，它就重定向到活动主题。

## 4.为徽标添加动画

现在，当启动画面结束时，我们将为徽标添加一些动画，这样它就以动画结束了。

```
*installSplashScreen*().*apply* **{** setOnExitAnimationListener **{** sp **->** *// Create your custom animation.* sp.iconView.animate().rotation(180F).*duration* = 3000L
        val slideUp = ObjectAnimator.ofFloat(
            sp.iconView,
            View.*TRANSLATION_Y*,
            0f,
            sp.iconView.*height*.toFloat()
        )
        slideUp.*interpolator* = AnticipateInterpolator()
        slideUp.*duration* = 3000L

        *// Call SplashScreenView.remove at the end of your custom animation.* slideUp.*doOnEnd* **{** sp.remove() **}** *// Run your animation.* slideUp.start()
    **}**
```

## 5.等待从 api 检索数据

有时我们会在 MainActivity 中检索数据，我们需要显示闪屏，直到从服务器中获取数据。用于改善用户体验。在 setKeepOnScreenCondition { }中添加您的条件。当该条件为真且满足时，将显示主屏幕。

```
setKeepOnScreenCondition **{** // Your condition
**}**
```

这就是实现闪屏的新方法。

谢谢大家！..