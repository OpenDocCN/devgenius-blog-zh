# Android 上的 d(原生类 Java)编程

> 原文：<https://blog.devgenius.io/d-c-evolution-programming-on-android-9bdf9aa1b67d?source=collection_archive---------8----------------------->

![](img/fb9b5cf066dd6ac5805502adbe5309e5.png)

来自 dlang.org 的照片

# Android 上的 D-Lang

创建这个库是因为在查看 D-Lang 官方维基时出现了信息不匹配，以[为 Android](https://wiki.dlang.org/Build_D_for_Android) 构建 D

主要的区别是，我展示了如何为您的环境获得正式的 LDC 版本，如何在 Linux 上设置您的变量，并且我发现了一个问题，就是在 ldc2.conf 中的 lib-dirs 后面缺少了一个/lib

# 安装 Android Studio 并获得 NDK

这是一个关于如何做的非常简单的教程，因为它有关于如何获得 android 的 NDK 的无数教程。在 https://developer.android.com/studio[获得 Android Studio](https://developer.android.com/studio)完成安装进入你的 Android Studio IDE(任务栏)的右上角并搜索`SDK Manager`安装目标 SDK，目前最新的是 Android 10(Q)，它在“SDK 平台”旁边的“SDK 平台”选择中，它有“SDK 工具”，点击它并选择 NDK。 Android SDK Tools，Android SDK Platform-Tools，Android SDK Build-Tools 然后点击窗口顶部的 OK，上面有 SDK 所在的位置，你可以随时再查看一遍以供参考，记住你的 ndk 安装在哪里就行了

## 在 Linux 上安装 LDC

对于在 Linux 上安装 LDC，我不建议从仓库中获取，而是去 Dlang 官方安装网站: [D-Lang 官方安装网站](https://dlang.org/install.html)然后获取 D-Lang 官方安装脚本(Install . sh)——>直接链接 [D-Lang 安装脚本](https://dlang.org/install.sh)之后打开 Bash 并:

*   chmod +x ./install.sh
*   。/install.sh ldc 执行这些命令会将它安装在您的/home/currentUser/dlang(~/dlang)中。如果您尝试执行 than ldc2(当前版本)，您会看到它不可用，安装脚本实际上附带了一个参数，该参数允许您“激活”(实际上将重要变量导出到您的路径中)，但由于某种原因，我的 Linux not exports 命令没有保存这些变量。所以我们要做的很简单:
*   前往~/dlang/ldc(当前版本)
*   用你喜欢的文本编辑器打开激活
*   您将在文本编辑器中找到以下几行

重要的行是那些在`if [ -v _OLD_D_PATH ]`后面有`export`的行，表示更容易理解:

用您当前的用户名更改/hipreme/

*   去你的$家
*   编辑。bashrc(可以随便 sudo vim ~/。如果你愿意的话)
*   在导出的顶部，复制上面描述的每一行
*   现在你的电脑应该可以运行 ldc 了

## 为 LDC 获取 Android 库

现在你需要在你的 LDC 上包含一个 android 架构库，要做到这一点，进入 [LDC github 库](https://github.com/ldc-developers/ldc/)进入[发布版](https://github.com/ldc-developers/ldc/releases/)寻找一个与你的 LDC 版本匹配的发布版，在我写这篇文章的时候，我的版本是 1.22.0，所以，它会有前缀像**LDC 2–1 . 22 . 0-Android**你要搜索的是 Android 架构 64 (aarch64)，所以我需要下载的版本是:

***重要***

> *如果您的目标架构没有预构建的二进制文件(LDC 开发人员发布资产中的那些)，您将不得不[自己构建 Phobos 和 DRuntime](https://wiki.dlang.org/Building_LDC_runtime_libraries)*
> 
> *请注意，在您的架构目标文件夹中，您会看到有一个 lib/和其他 lib(通常与 32 位相关)，您可以使用它为另一个目标进行分发，在 aarch64 中有一个 x86_64，您可以设置一个针对它的配置，在-gcc 中的二进制文件中，搜索 x86_64*

*下载之后，是时候设置你的编译器来找到它的存在了。*

## *添加编译命令*

*   *前往~/dlang/ldc(yourVersionHere)*
*   *提取您新获得的文件*
*   *将其重命名为`lib-android__aarch64`*
*   *前往~/dlang/LDC(your version here)/etc/*
*   *打开 ldc2.conf*
*   *将以下内容附加到文件末尾:*

```
*"aarch64-.*-linux-android":
{
    switches = [
        "-defaultlib=phobos2-ldc,druntime-ldc",
        "-link-defaultlib-shared=false",
        "-gcc=/home/hipreme/Android/Sdk/ndk/21.3.6528147/toolchains/llvm/prebuilt/linux-x86_64/bin/aarch64-linux-android21-clang",
    ];
    lib-dirs = [
        "%%ldcbinarypath%%/../lib-android_aarch64/lib",
    ];
    rpath = "";
};*
```

*   *在`-gcc=`一行，你应该放上你的安卓 NDK，我的在那个文件夹里，但是你应该搜索的是`toolchains/llvm/prebuilt/linux-x86_64/bin/aarch64-linux-android21-clang`*

> *如果你正在为 Windows 编译，你的路径可能类似于`"-gcc=C:/Users/Hipreme/AppData/Local/Android/Sdk/ndk/21.3.6528147/toolchains/llvm/prebuilt/windows-x86_64/bin/aarch64-linux-android21-clang.cmd"`*

*之后，从官方 wiki 下载一个示例 d 程序:*

*如果它编译得好，您的 LDC 就被正确地配置为开始与 Android 一起工作。*

*在进入本教程的下一部分之前，我想补充一点，这个架构是作为一个示例使用的，因为这是我的手机各自的架构，如果你想编译任何其他架构，看看[与 LDC 的交叉编译](https://wiki.dlang.org/Cross-compiling_with_LDC)*

# *为 Android 准备您的 D 模块*

*在你第一次成功地将它编译到 Android 架构后，你实际上需要做三件事:*

1.  *您需要在您的文件顶部导入 D 模块 JNI:`import jni`。JNI 模块不是我做的，为了保持兼容性，这个库中的文件是对[原始库](https://github.com/Diewi/android)的引用*
2.  *创建一个您希望导出到 Android 的文件。您需要将您的函数作为`extern(C)`导出，并按照与命名从 NDK Java _ package name _ class name _ method name(jniev * env，jclass clazz) sample.d 导出的 C 函数相同的约定命名它*

1.  *将它编译成一个共享库，假设你的目标是 Arm64 架构，你需要调用:`ldc2 -mtriple=aarch64--linux-android --shared sample.d`这将输出 libsample.so，这个文件将包含在你的 Android 项目中*

# *一句警告*

*总是调用 rt_init，否则很可能会导致 sigsegfault(5)，使用 to！string 直到我调用 rt_init()才起作用；*

# *创建 Android 项目*

*在撰写本文时，在构建 android 的官方 D Wiki 中，记录了使用 Ant 生成 apk 的方法，但这已经过去很久了，几乎没有关于如何实际使用它的文档，而且它对今天的 Android 开发毫无用处，因为每个人都使用 gradle，在管理整个 Android 项目时对 Gradle 了解很少，它可以通过简单的命令添加许多库，所以我们将使用 Android Studio*

*   *打开 Android Studio*
*   *进入“开始一个新的 Android 工作室项目”*
*   *选择“空活动”*
*   *命名您的项目(假设我将我的项目命名为 Test，并将包命名为 com.hipreme.test)*
*   *配置并点击 Finish(我目前使用的是 min API 23)*
*   *进入窗口左侧，将视图设置为“项目”。文件夹结构必须如下所示:*

```
*Test
|--.gradle
|--.idea
|--app
|--|--build
|--|--libs
|--|--src
|--|--|--androidTest
|--|--|--main
|--|--|--test
|--|--|.gitignore
|--|--app.iml
|--|--build.gradle
|--|--proguard-rules.pro
|--gradle
|--.gitignore
|--buid.gradle
|--gradle.properties
|--gradlew
|--gradlew.bat
|--local.properties
|--settings.gradle
|--Test.iml*
```

*最后还有:*

```
*|> External Libraries
|> Scratches and Consoles*
```

*打开 Test/app/src/main 文件夹，在 main 里面创建一个名为`jniLibs`的文件夹，这个文件夹是**非常重要的**，它是默认的文件夹，用来存放你的共享库和你的. apk 一起导入，如果你想用其他的名字，你需要改变你的 gradle 文件。为了将您的库放入该文件夹中，您实际上需要为目标体系结构创建目录，因此，在其中创建:*

*   *armeabi-v7a(为此，常用的是 ldc2(版本)-android-armv7a/lib*
*   *arm64-v8a(这是我们现在的目标，ldc2(版本)-android-aarch64/lib)*
*   *x86(就是 armv7a 的 lib 32-> LDC 2(版本)-android-armv7a/lib686*
*   *x86_64(是 aarch64 的 lib 32-> LDC 2(版本)-android-aarch64/lib-x86_64 参考，查看 android 官方网站 ndk abi 指南: [Android ABI 指南](https://developer.android.com/ndk/guides/abis)你的新主结构必须是:*

```
*main
|--java
|--jniLibs
|--|--armeabi-v7a
|--|--arm64-v8a
|--|--x86
|--|--x86_64
|--res
|--AndroidManifest.xml*
```

*创建这些文件夹后，你可以将你的共享库移动到其中一个文件夹中，正如我们用 aarch64 展示的，你应该将你的 libsample.so 移动到 arm64-v8a 中。这样，你现在应该能够通过调用你的 java 代码将你的库导入 Android Studio，所以，在 main/ảva/your/long/package/name，创建一个新的。java 文件，我将创建一个 Sample.java 文件:*

*现在，你可以从代码中的任何地方调用`Sample.methodname("hello")`, D 将被调用*

## *一些有用的库*

*   *log 是一个调用 android 登录函数的库，例如:`__android_log_vprint`*

*如果你想让 SDL 和 D 在一起，在某个地方提到我，我会把它贴在这里！*