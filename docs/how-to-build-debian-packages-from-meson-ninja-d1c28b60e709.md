# 如何用介子/忍者构建 Debian 包

> 原文：<https://blog.devgenius.io/how-to-build-debian-packages-from-meson-ninja-d1c28b60e709?source=collection_archive---------0----------------------->

![](img/df91098588af5d9f4e9c8731ed1f3f61.png)

## 最少二进制文件…

**作者注:** *对于我的老读者，对不起，我知道这与我通常的内容不同，我应该是一名硬件工程师，但这太令人沮丧了，我不得不自己解决它，所以我给 stack overflow-answers 和 medium-blog-posts-on-very-specific-software-issues 的作者回信，因为我已经读了很多。另外，对于硬件工程师来说，处理 rando 固件问题是一件非常频繁的事情。当然，我也不是出于同样的原因付费，在阅读了许多试图解决类似问题的类似帖子后，我想回馈社区。开源万岁(是的，我知道，整件事充满了讽刺)。*

*我知道世界上只有 5 个人会欣赏这一点，但对这些人来说，这意味着全世界，就像类似的帖子对我的影响一样*😅*哇，社区❤*

也许你和我一样，是一名机械工程师或机电工程师，经常被卷入固件任务。假设有人给了你一个用 meson/ninja 构建的漂亮的 lil 软件包，但是*哦不，*你至少需要一个. deb 二进制包来实现版本控制/易于安装等等。

我这么说既是为了给我的创业伙伴一个大大的互联网拥抱，也是为了说明我现在没有也从来没有声称自己是一个真正的软件工程师，所以这是为了让你从 0 到 1，而不是从 1 到 100。明白了吗？

不建议用于生产(但是我们都已经将类似这样的东西投入生产，不要说谎。)

# 从介子/忍者构建 Debian 二进制文件

最不幸的是，这将比`checkinstall -i`或`make -j4`更加手动。

我强烈推荐阅读这篇[Unix&Linux stack exchange 帖子](https://unix.stackexchange.com/questions/30303/how-to-create-a-deb-file-manually)，我把它收藏了起来，并在试图解决这个问题时没完没了地翻来覆去。它只是关于如何构建 Debian 软件包，我对此一无所知(它很短，不像我的漫谈):

[](https://unix.stackexchange.com/questions/30303/how-to-create-a-deb-file-manually) [## 如何手动创建 DEB 文件？

### 我的建议是做一个源码包。安装 build-essential，debhelper，dh-make。转到目录…

unix.stackexchange.com](https://unix.stackexchange.com/questions/30303/how-to-create-a-deb-file-manually) 

## 0.继续使用 gechyo Debian 构建工具…

```
sudo apt-get install debhelper build-essentials dh-make
```

1.  然后，确保你已经用`meson build`构建了你的包。
2.  如果您还没有，`cd`进入您的目录——这是您的`meson.build`所在的顶层文件夹，如果您最初在原型制作时做了类似于`meson build --prefix=/somewhere/else/`的事情，不一定是您的可执行文件所在的位置。
3.  现在，你需要`dh_make --createorig`。

用“s”表示单个包装，用“Y”表示接受。

Debian 希望你的包名有一定的格式。如果文件夹名称格式不正确或出现以下错误:

```
For dh_make to find the package name and version, the current directory
needs to be in the format of <package>-<version>.  Alternatively use the -p flag using the format <name>_<version> to override it.
The directory name you have specified is invalid!
```

那么你应该做类似于`dh_make --createorig -p example_0.0.1`的事情

4.运行`dh_auto_configure --buildsystem=meson`，等待过程结束。这是支持 meson/ninja 构建系统的一个关键特性，它最近才被整合到 debhelper 12 中，所以如果有错误，请确保您拥有正确的 debhelper 版本。

5.`dh_make`生成了`debian/control /compat`等。包构建过程的文件。您可能希望在 auto_configure 之后编辑它们。关于何时编辑`debian/rules`，请参见下面的“故障排除”。

6.最后，您已经为`dpkg-buildpackage -rfakeroot -us -uc -b`构建。deb(`-b`标志只构建二进制文件)。

![](img/9da525523affb3014a6cffe47d29460e.png)

我无耻地从:[https://medium . com/@ galaktyk 01/how-to-build-opencv-with-gstreamer-b 11668 fa 09 c](https://medium.com/@galaktyk01/how-to-build-opencv-with-gstreamer-b11668fa09c)那里窃取了这个想法

# 尽情享受吧！

你做到了！恭喜你。包装是一门神奇的黑暗艺术，我还没有完全理解。

请注意，这将设置安装类似于`dpkg -i examplepack_0.0.1-arch.deb`的目录，并将东西放入`/usr/local`。

如果您尝试`dpkg -x`到一个本地文件夹，可执行文件将无法正常运行，即使您的初始 meson/ninja 目录可能已经配置为在本地运行。因为这个小小的差异，我认为我已经错误地做了整整两天了。

希望我达到了自己的标准:

# 附录:故障排除

如果测试和依赖项检查失败，您可能希望通过重写两者来强制构建。有时会发生这种情况*特别是*当软件是在 PC 上构建的，但您的目标系统是嵌入式处理器时——当某些硬件不存在时，某些测试可能会失败(如显示器/摄像头检查)。对于自定义版本的库，依赖项检查也可能会失败。

这是绝对不建议生产，但嘿，世界是你的燃烧。

您可以通过将上面的#5 更改为:

`DEB_BUILD_OPTIONS='nocheck' dpkg-buildpackage -rfakeroot -us -uc -b`

您可以通过格式化您的`rules`文件来覆盖依赖性检查，如下所示:

```
%:
 dh $@ override_dh_shlibdeps: 
      dh_shlibdeps --dpkg-shlibdeps-params=--ignore-missing-info
```

不要复制/粘贴上面的内容，因为`rules`很挑剔，你必须确保`dh_shlibdeps`前面的空格实际上是一个单独的 **tab** 键——你可能还需要删除任何 **enter** 键的空格(我认为 **shift+enter** 应该没问题)。如果遇到与`debian/rules`相关的错误，不要害怕尝试几次重新格式化空白。如果您没有编辑由`dh_make`生成的文件，它可能看起来更像这样:

```
# see ENVIRONMENT in dpkg-buildflags(1)
# package maintainers to append CFLAGS
#export DEB_CFLAGS_MAINT_APPEND  = -Wall -pedantic
# package maintainers to append LDFLAGS
#export DEB_LDFLAGS_MAINT_APPEND = -Wl,--as-needed%:
 dh $@# dh_make generated override targets
# This is example for Cmake (See [https://bugs.debian.org/641051](https://bugs.debian.org/641051) )
#override_dh_auto_configure:
# dh_auto_configure -- # -DCMAKE_LIBRARY_PATH=$(DEB_HOST_MULTIARCH)
override_dh_shlibdeps: 
   dh_shlibdeps --dpkg-shlibdeps-params=--ignore-missing-info
```