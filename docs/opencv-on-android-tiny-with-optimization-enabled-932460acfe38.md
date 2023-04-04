# Android 上的 OpenCV 小巧且支持优化

> 原文：<https://blog.devgenius.io/opencv-on-android-tiny-with-optimization-enabled-932460acfe38?source=collection_archive---------0----------------------->

![](img/4d2b06fed8fb0efd8fa44ee1825271e8.png)

Android 上 OpenCV 的子集

> 这篇文章也发表在 [LearnOpenCV](https://www.learnopencv.com/install-opencv-on-android-tiny-and-optimized/) 上

*我的 OpenCV Android SDK =* ***小尺寸*** *库*

如果您选择 OpenCV 用于生产，您的主要目标是减小库的大小，并使其具有更好的性能。OpenCV 是一个拥有大量算法的令人敬畏的库，但是你必须在你的应用程序中使用这些算法的一个非常小的子集，因此包含需要的而省略其余的是非常有意义的。

[库](http://www.yolinux.com/TUTORIALS/LibraryArchives-StaticAndDynamic.html)可以和你的应用代码一起静态编译，也可以在运行时动态链接，这完全是应用开发的决定。这里我们将创建共享对象(。所以)对于这个演示。

Android 是一个多功能的操作系统，可以在多种硬件上运行，从手机到 IOT 设备，Raspberry Pi，再到各种单板计算机，因此对称为 is a 的特定计算机架构的代码进行交叉编译变得非常重要。因为我们正在为 Android 构建它，所以我们需要它的内置工具。Android 的构建工具有一个好听的名字，叫做 NDK(原生开发工具包)，但这正是我们所需要的。

以下是我们将要使用的软件及其各自版本的列表:

1.  *cmake — 3.7.2*
2.  *NDK — r14b*
3.  *OpenCV — 3.4.1*
4.  *主机— Ubuntu — 16.4*
5.  *目标——ARM eabi-v7a(基于 ARM)*
6.  *Android SDK*[*25 . 2 . 5 工具*](https://dl.google.com/android/repository/tools_r25.2.5-linux.zip) *。*

## 第一步:

下载 NDK 并解压到你的工作区。NDK 有不同的版本，因此阅读发行说明并选择你的项目需要的版本是很重要的。但是在这个练习中，它的 [Android NDK 版本 14b](https://dl.google.com/android/repository/android-ndk-r14b-linux-x86_64.zip) 在 Docker 容器中的 Ubuntu 上。但是这可以在任何 Linux 机器上。

```
root@dc:/opt/android-ndk-r14b# **ls**
CHANGELOG.md ndk-build ndk-gdb ndk-which prebuilt shader-tools source.properties sysroot
build ndk-depends ndk-stack platforms python-packages simpleperf sources toolchains
```

我们将为 OpenCV 编译构建独立的工具链，

```
root@dc:/opt/android-ndk-r14b# **./build/tools/make_standalone_toolchain.py \
--arch arm \
--api 23 \
--install-dir /tmp/my-android-toolchain**
```

该命令将基于我的项目所需的`--arch`和`--api`创建工具链，并将它们安装在`/tmp/my-android-toolchain`目录中。

## 第二步:

通常，ANDROID_NDK 路径被设置为预构建的工具链，它与 NDK 一起提供。但是我们将使用我们在上一步中构建的环境变量，我们需要让`cmake`知道这个重要的事实，从而设置环境变量。

```
$ export ANDROID_STANDALONE_TOOLCHAIN=/tmp/my-android-toolchain/
```

这是我们的构建环境需要知道的，不需要其他变量。如果你已经有安卓 _NDK 设置，请取消设置。

## 第三步:

为 Android 构建还需要几个重要的包，叫做 Ninja 和 ant，我们也会安装它们:

```
$ sudo apt-get install ninja-build ant
```

## 第四步:

下载 OpenCV，没有比 Satya Mallick 的博客更好的地方可以找到下载步骤了。你可以按照博客建议的方式编译，也可以下载所有的包，OpenCV repository，然后把它放在那里。因为我们正在为 Android 编译，所以我们可以跳过博客中提到的编译部分。如果你需要`contrib`模块，为这篇文章下载它们，

不管怎样，让我们转到 OpenCV 目录。

```
$ cd opencv/
$ mkdir build
$ cd build
$ cmake  \
-DCMAKE_TOOLCHAIN_FILE=../platforms/android/android.toolchain.cmake\
-DANDROID_STL=gnustl_shared \
-DANDROID_NATIVE_API_LEVEL=23 ..
```

***注意****:OpenCV 3.0 如果 SDK 高于 25.2.5，则使失败。请确保您安装了合适的 Android SDK。*

输出将显示此构建的配置，这是您可以更改您喜欢的配置的步骤。

对于基于 ARM 的设备，我通常使用以下方法进行优化:

1.  **T21 霓虹**
2.  ***VFPV3***

查看`platforms/android/android.toolchain.cmake`的序言，了解各种配置选项。

```
$ make -j $nproc
```

## 第五步:

希望构建进展顺利，并且您已经准备好了 [ELF](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format) 以`.so`(共享对象)的形式输出，您也可以生成输出为`.a`，为此，您需要在`cmake`中启用适当的标志。输出如下所示:

```
root@dc:/opencv/build/lib/armeabi-v7a# ls
libopencv_calib3d.a libopencv_flann.a **libopencv_java3.so** libopencv_shape.a libopencv_video.a
libopencv_core.a libopencv_highgui.a libopencv_ml.a libopencv_stitching.a libopencv_videoio.a
libopencv_dnn.a libopencv_imgcodecs.a libopencv_objdetect.a libopencv_superres.a libopencv_videostab.a
libopencv_features2d.a libopencv_imgproc.a libopencv_photo.a libopencv_ts.a
```

`libopencv_java3.so`是你的 android 项目所需要的，但是如果你看看它的大小，它大约有 **9.5MB** :

```
root@dc:/opencv/build/lib/armeabi-v7a# du -h libopencv_java3.so
**9.4M** libopencv_java3.so
```

这是因为，它包含了所有的模块。要验证这一点，请运行以下命令:

```
$ strings -a libopencv_java3.so | grep .cpp
```

## 第六步:

让我们根据我们的需要修整图书馆。假设我们只需要两个模块`core`和`imageproc`:

```
root@dc:/opencv/build/lib/armeabi-v7a# /tmp/my-android-toolchain/bin/arm-linux-androideabi-g++ \
-shared -o **libopencv_tiny.so** \
--sysroot=/tmp/my-android-toolchain/sysroot/ \
-Wl,--whole-archive **libopencv_core.a** \
**libopencv_imgproc.a** -Wl,--no-whole-archive
```

现在检查`libopencv_tiny.so`的文件类型和大小

```
root@dc:/opencv/build/lib/armeabi-v7a# file libopencv_tiny.so
libopencv_tiny.so: ELF 32-bit LSB shared object, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /system/bin/linker, **not stripped**root@dc:/opencv/build/lib/armeabi-v7a# du -h libopencv_tiny.so
**8.3M libopencv_tiny.so**
```

这是 ***无条纹*** ，让我们把它剥开:

```
root@dc:/opencv/build/lib/armeabi-v7a# /tmp/my-android-toolchain/bin/arm-linux-androideabi-strip --strip-unneeded libopencv_tiny.soroot@dc:/opencv/build/lib/armeabi-v7a# file libopencv_tiny.so
libopencv_tiny.so: ELF 32-bit LSB shared object, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /system/bin/linker, **stripped**root@dc:/opencv/build/lib/armeabi-v7a# du -h libopencv_tiny.so
**3.4M libopencv_tiny.so**
```

## 可选步骤:“奖励回合”

您可以进一步向下钻取每个`.o`文件，而不是`.a`文件，这可以通过使用`ar`工具来实现:

```
$ /tmp/my-android-toolchain/bin/arm-linux-androideabi-ar x  libopencv_core.a
```

`ar`工具从`.a`文件中提取所有的`.o`，然后你可以用我们之前创建的方法从它们中创建`.so`，命令如下:

```
root@dc:/opencv/build/lib/armeabi-v7a# /tmp/my-android-toolchain/bin/arm-linux-androideabi-g++ \
-shared -o **libopencv_tiny.so** \
--sysroot=/tmp/my-android-toolchain/sysroot/ \
-Wl, — whole-archive ***.o** -Wl, — no-whole-archive
```

## 第七步:创建。所以对于您的应用程序

联系是努力的终点，让我们也跨越这个里程碑。由于这是针对特定平台的交叉编译，所以在`.so`中包含一些提取库是很重要的，这些额外的库完全是第三方的和特定于平台的。如果您不包括它们，您将得到一个链接错误，因此让我们这样做:

```
root@dc:/opencv/build/lib/armeabi-v7a# /tmp/my-android-toolchain/bin/arm-linux-androideabi-g++ -L /opencv/build/3rdparty/lib/armeabi-v7a  **-lz -llog -ljnigraphics -ltegra_hal -lcpufeatures -lm -lgnustl_shared** -fexceptions -frtti -fpic  -shared -Wl,-soname,libopencv_java.so -o libopencv_java.so  -Wl,--whole-archive libopencv_core.a libopencv_imgproc.a  -Wl,--no-whole-archive
```

依赖项应该如下所示，这些额外的库很少是`.a`，所以它们不会出现在依赖项中。

```
root@dc:/opencv/build/lib/armeabi-v7a# readelf -d libopencv_tiny.soDynamic section at offset 0x3615d8 contains 33 entries:
  Tag        Type                         Name/Value
 0x00000003 (PLTGOT)                     0x362b18
 0x00000002 (PLTRELSZ)                   2488 (bytes)
 0x00000017 (JMPREL)                     0x3f674
 0x00000014 (PLTREL)                     REL
 0x00000011 (REL)                        0x28cb4
 0x00000012 (RELSZ)                      92608 (bytes)
 0x00000013 (RELENT)                     8 (bytes)
 0x6ffffffa (RELCOUNT)                   11570
 0x00000006 (SYMTAB)                     0x148
 0x0000000b (SYMENT)                     16 (bytes)
 0x00000005 (STRTAB)                     0xaf78
 0x0000000a (STRSZ)                      97135 (bytes)
 0x00000004 (HASH)                       0x22ae8
 0x00000001 (NEEDED)                     Shared library: [libz.so]
 0x00000001 (NEEDED)                     Shared library: [liblog.so]
 0x00000001 (NEEDED)                     Shared library: [libjnigraphics.so]
 0x00000001 (NEEDED)                     Shared library: [libm.so]
 0x00000001 (NEEDED)                     Shared library: [libc.so]
 0x00000001 (NEEDED)                     Shared library: [libdl.so]
 0x0000000e (SONAME)                     Library soname: [libopencv_tiny.so]
 0x0000001a (FINI_ARRAY)                 0x35e7b4
 0x0000001c (FINI_ARRAYSZ)               8 (bytes)
 0x00000019 (INIT_ARRAY)                 0x362554
 0x0000001b (INIT_ARRAYSZ)               132 (bytes)
 0x00000010 (SYMBOLIC)                   0x0
 0x0000001e (FLAGS)                      SYMBOLIC BIND_NOW
 0x6ffffffb (FLAGS_1)                    Flags: NOW
 0x6ffffff0 (VERSYM)                     0x27690
 0x6ffffffc (VERDEF)                     0x28c58
 0x6ffffffd (VERDEFNUM)                  1
 0x6ffffffe (VERNEED)                    0x28c74
 0x6fffffff (VERNEEDNUM)                 2
 0x00000000 (NULL)                       0x0
```

在检查尺寸之前，确保您新创建的`.so``strip`。

现在，您的共享对象(`libopencv_tiny.so`)已经为 android 部署做好了准备。它通常被命名为`libopencv_java3.so`，其中`3`是版本，但不要担心它只是一个文件名，它仍然可以工作，或者使其符合 android 要求，并将其命名为`libopencv_java3.so`。

## 步骤 8:用示例代码链接

假设我们在目录`./demo`内的文件`demo.cpp`中有下面的代码，您想要链接到创建的`.so`:

```
#include "opencv2/opencv.hpp"
#include<iostream>int main()
{
    std::cout << "OpenCV Version " << CV_VERSION << std::endl;
}
```

我们必须使用相同的工具链来编译和链接`demo.cpp`，你能猜到为什么吗？

好吧！既然你在思考，让我快速开始编译:

```
root@dc: /tmp/my-android-toolchain/bin/arm-linux-androideabi-g++ -L/demo  -I /demo/include/ -Wall    -o demo demo.cpp -lopencv_tiny
```

我已经把生成的`libopencv_tiny.so`移到了文件夹`./demo`和所有的 opencv `includes`(我的意思是。hpp)，这只是为了证明我们是孤立的，没有使用`/usr/local/include`

> 现在，回到我的问题，为什么是相同的工具链？

```
root@5c7fe9a72d74:/demo# readelf -h libopencv_tiny.so
ELF Header:
  Magic:   7f 45 4c 46 01 01 01 00 00 00 00 00 00 00 00 00
  Class:                             ELF32
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              DYN (Shared object file)
  **Machine:                           ARM**
  Version:                           0x1
  Entry point address:               0x0
  Start of program headers:          52 (bytes into file)
  Start of section headers:          3561860 (bytes into file)
  Flags:                             0x5000200, Version5 EABI, soft-float ABI
  Size of this header:               52 (bytes)
  Size of program headers:           32 (bytes)
  Number of program headers:         8
  Size of section headers:           40 (bytes)
  Number of section headers:         27
  Section header string table index: 26
```

你拿到了吗？好吧，让我这么说:`.so`是为一个特定的`arch`类型交叉编译的，那就是`ARM`，其中我们的主机是基于 linux 的`x86`。这就是为什么，您只需要在相同的目标机器类型上运行可执行文件。

## 最后一步:

如果你在 x86 上构建它，你还必须告诉你的`.so`驻留在哪里，你可以把它放在你喜欢的任何地方，我的意思是在你的项目或工作区的任何文件夹中，因为这个运行时链接，`ld`应该能找到它。最好的方法是更新环境变量:

```
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:<your .so path>
```

现在，说你变得很大方，想分享给系统上的每个人。为此你需要有`root`权限。您需要它有两个原因:

*   将库放在标准的系统位置，可能是`/usr/lib`或`/usr/local/lib`，普通用户对其没有写权限。
*   您需要修改`ld.so`配置文件和缓存。如`root`所示，执行以下操作:

```
cp /demo/libopencv_tiny.so /usr/local/lib
chmod 0755 /usr/local/lib/libopencv_tiny.so
```

我们需要通知`ld`它可以使用了，因此让我们更新缓存:

```
ldconfig
```

现在，我们不需要`LD_LIBRARY_PATH`，所以让我们也清除它。注意:你只能从`LD_LIBRARY_PATH`开始清除自己的路径。

```
unset LD_LIBRARY_PATH
```

## 另一种选择:使用 OpenCV Makefile 编译

我在此附上由 [Yk 订单](https://medium.com/u/553a25b3deaf?source=post_page-----932460acfe38--------------------------------)建议的回复。使用 OpenCV `Makefile,`的好处是所有的依赖都被考虑到了，你需要为你的平台传递合适的构建工具。

Android 示例:

```
cmake -DCMAKE_TOOLCHAIN_FILE=../platforms/android/android.toolchain.cmake \
-D ANDROID_ABI=arm64-v8a \
-D CMAKE_BUILD_TYPE=Release \
-D ANDROID_NATIVE_API_LEVEL=23 \
-D WITH_CUDA=OFF \
-D WITH_MATLAB=OFF \
-D BUILD_ANDROID_EXAMPLES=OFF \
-D BUILD_DOCS=OFF \
-D BUILD_PERF_TESTS=OFF \
-D BUILD_TESTS=OFF \
-D ANDROID_STL=c++_shared \
-D BUILD_SHARED_LIBS=ON \
-D BUILD_opencv_objdetect=OFF \
-D BUILD_opencv_video=OFF \
-D BUILD_opencv_videoio=OFF \
-D BUILD_opencv_features2d=OFF \
-D BUILD_opencv_flann=OFF \
-D BUILD_opencv_highgui=OFF \
-D BUILD_opencv_ml=OFF \
-D BUILD_opencv_photo=OFF \
-D BUILD_opencv_python=OFF \
-D BUILD_opencv_shape=OFF \
-D BUILD_opencv_stitching=OFF \
-D BUILD_opencv_superres=OFF \
-D BUILD_opencv_ts=OFF \
-D BUILD_opencv_videostab=OFF \
..
```

在您的 Linux 或 MacOS 上:(它将在构建中包含`imgproc and highgui`模块)

```
cmake
-D CMAKE_BUILD_TYPE=Release \
-D WITH_CUDA=OFF \
-D WITH_MATLAB=OFF \
-D BUILD_ANDROID_EXAMPLES=OFF \
-D BUILD_DOCS=OFF \
-D BUILD_PERF_TESTS=OFF \
-D BUILD_TESTS=OFF \
-D BUILD_SHARED_LIBS=ON \
-D BUILD_opencv_objdetect=OFF \
-D BUILD_opencv_video=OFF \
-D BUILD_opencv_videoio=OFF \
-D BUILD_opencv_features2d=OFF \
-D BUILD_opencv_flann=OFF \
-D BUILD_opencv_ml=OFF \
-D BUILD_opencv_photo=OFF \
-D BUILD_opencv_python=OFF \
-D BUILD_opencv_shape=OFF \
-D BUILD_opencv_stitching=OFF \
-D BUILD_opencv_superres=OFF \
-D BUILD_opencv_ts=OFF \
-D BUILD_opencv_videostab=OFF \
-D BUILD_opencv_highgui=ON \
-D BUILD_opencv_imgproc=ON \
..
```

你可以在最终版本中选择你需要的模块，事实上我可能漏掉了一些。

> *享受动态链接的力量！*

**你可以在|**[**LinkedIn**](https://www.linkedin.com/in/mdeore/)**|**[**网站**](https://tomdeore.wixsite.com/epoch)**|**[**Github**](https://github.com/milinddeore)**|**