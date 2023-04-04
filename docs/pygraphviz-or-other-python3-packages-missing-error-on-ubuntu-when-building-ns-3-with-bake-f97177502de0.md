# 在 Ubuntu 上使用 Bake 构建 ns-3 时，pygraphviz 和其他 python 3 包丢失错误

> 原文：<https://blog.devgenius.io/pygraphviz-or-other-python3-packages-missing-error-on-ubuntu-when-building-ns-3-with-bake-f97177502de0?source=collection_archive---------4----------------------->

注意:我现在使用的是 Ubuntu 18.04，但我认为这将适用于任何版本的 Ubuntu，因为所有的 Linux 操作系统似乎都支持符号链接，而且这是一个非常老的特性。

先决条件:

我已经安装了烘焙，并且正在使用

```
bake.py show 
```

向我展示我是否拥有 ns-3 所需的所有依赖项

好了，下面是完整的错误信息:

```
bake.py show
module: setuptools (enabled)
  No dependencies!
module: python3-setuptools (enabled)
  No dependencies!
module: gi-cairo (enabled)
  No dependencies!
module: gir-bindings (enabled)
  No dependencies!
module: pygobject (enabled)
  No dependencies!
module: pygraphviz (enabled)
  No dependencies!
module: python3-dev (enabled)
  No dependencies!
module: python-dev (enabled)
  No dependencies!
module: qt (enabled)
  No dependencies!
module: g++ (enabled)
  No dependencies!
module: mercurial (enabled)
  No dependencies!
module: pybindgen (enabled)
  depends on:
     python3-dev (optional:True)
     python3-setuptools (optional:False)
module: pyviz-gtk3-prerequisites (enabled)
  depends on:
     python3-dev (optional:True)
     pygraphviz (optional:True)
     pygobject (optional:True)
     gi-cairo (optional:True)
     gir-bindings (optional:True)
module: netanim-3.108 (enabled)
  depends on:
     qt (optional:False)
     g++ (optional:False)
module: netanim (enabled)
  depends on:
     qt (optional:False)
     g++ (optional:False)
module: pybindgen-0.19.0.post4+ng823d8b2 (enabled)
  depends on:
     python-dev (optional:True)
     setuptools (optional:False)
module: pybindgen-0.22.0 (enabled)
  depends on:
     python3-dev (optional:True)
     setuptools (optional:False)
module ns: ns-3.29 (enabled)
  depends on:
     netanim-3.108 (optional:True)
     pybindgen-0.19.0.post4+ng823d8b2 (optional:True)
     pyviz-gtk3-prerequisites (optional:True)
     mercurial (optional:False)
module: ns-3-dev (enabled)
  depends on:
     netanim (optional:True)
     pybindgen (optional:True)
     pyviz-gtk3-prerequisites (optional:True)
module ns: ns-3.34 (enabled)
  depends on:
     netanim-3.108 (optional:True)
     pybindgen-0.22.0 (optional:True)
     pyviz-gtk3-prerequisites (optional:True)-- System Dependencies --
 > g++ - OK
 > gi-cairo - OK
 > gir-bindings - OK
 > mercurial - OK
 > pygobject - OK
 > pygraphviz - Missing
   >> The pygraphviz is not installed, try to install it.
   >> Try: "sudo apt-get install python3-pygraphviz", if you have sudo rights.
 > python-dev - OK
 > python3-dev - OK
 > python3-setuptools - OK
 > qt - OK
 > setuptools - OK
```

基本上这已经很多了，但这是最重要的部分:

```
> pygraphviz - Missing
   >> The pygraphviz is not installed, try to install it.
   >> Try: "sudo apt-get install python3-pygraphviz", if you have
```

好吧，那么做他们建议的事情，他们显示以上表明，我已经安装了它:

```
Reading package lists... Done
Building dependency tree       
Reading state information... Done
python3-pygraphviz is already the newest version (1.4~rc1-1build2.1).
The following packages were automatically installed and are no longer required:
  efibootmgr fonts-liberation2 fonts-opensymbol gir1.2-geocodeglib-1.0
  gir1.2-gst-plugins-base-1.0 gir1.2-gstreamer-1.0 gir1.2-gudev-1.0 gir1.2-udisks-2.0
  grilo-plugins-0.3-base gstreamer1.0-gtk3 libcdr-0.1-1 libclucene-contribs1v5
  libclucene-core1v5 libcmis-0.5-5v5 libcolamd2 libdazzle-1.0-0 libe-book-0.1-1
  libedataserverui-1.2-2 libegl1-mesa libeot0 libepubgen-0.1-1 libetonyek-0.1-1
  libevent-2.1-6 libexiv2-14 libfreerdp-client2-2 libfreerdp2-2 libfwup1 libgee-0.8-2
  libgexiv2-2 libgom-1.0-0 libgpgmepp6 libgpod-common libgpod4 liblangtag-common
  liblangtag1 liblirc-client0 libllvm8 liblua5.3-0 libmediaart-2.0-0 libmspub-0.1-1
  libodfgen-0.1-1 libqqwing2v5 libraw16 librevenge-0.0-0 libsgutils2-2 libssh-4
  libsuitesparseconfig5 libvncclient1 libwayland-egl1-mesa libwinpr2-2 libxapian30
  libxmlsec1 libxmlsec1-nss lp-solve media-player-info python3-mako syslinux
  syslinux-common syslinux-legacy usb-creator-common
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

基本上就是说我已经安装了，0 安装了，0 升级了，所以我已经安装了，所以基本上就是说我已经安装了，但是 bake 就是找不到。

在你的 bake 目录的根目录下有一个名为 bakeconf.xml 的文件。打开它并搜索 pygraphviz，你会看到如下内容:

```
<module name="pygraphviz">
      <source type="system_dependency">
        <attribute name="dependency_test" value="(_gv.so or _graphviz.so or _graphviz.x86_64-linux-gnu.so or _graphviz.cpython-38-x86_64-linux-gnu.so or _graphviz.cpython-39-x86_64-linux-gnu.so or _graphviz.cpython-36m-x86_64-linux-gnu.so or /usr/lib/python3/dist-packages/pygraphviz/_graphviz.cpython-36m-x86_64-linux-gnu.so)"/>
        <attribute name="name_apt-get" value="python3-pygraphviz"/>
        <attribute name="name_yum" value="python3-pygraphviz"/>
        <attribute name="more_information" value="The pygraphviz is not installed, try to install it."/>
      </source>
      <build type="none" objdir="no">
      </build>
    </module>
```

在这个属性中,“dependency_test”是我们所关心的。上面有我们要找的文件的名字。我试图将路径硬编码到。所以在 pygraphviz 文件夹里。

顺便说一下，那个文件夹在这里:

```
/usr/lib/python3/dist-packages/pygraphviz
```

在该文件夹中执行“ls -halp ”,您将看到:

```
drwxr-xr-x   4 root root 4.0K Jan  6 12:51 ./
drwxr-xr-x 151 root root  12K Jan  6 12:16 ../
-rw-r--r--   1 root root  58K Nov  6  2016 agraph.py
-rw-r--r--   1 root root  67K Jun 27  2019 _graphviz.cpython-36m-x86_64-linux-gnu.so
-rw-r--r--   1 root root 7.9K Nov  6  2016 graphviz.i
-rw-r--r--   1 root root 8.7K Nov  6  2016 graphviz.py
-rw-r--r--   1 root root 187K Nov  6  2016 graphviz_wrap.c
-rw-r--r--   1 root root 1.8K Nov  6  2016 __init__.py
drwxr-xr-x   2 root root 4.0K Jan  5 18:06 __pycache__/
-rw-r--r--   1 root root 4.1K Nov  6  2016 release.py
drwxr-xr-x   3 root root 4.0K Jan  5 18:06 tests/
-rw-r--r--   1 root root  198 Nov  6  2016 version.py
```

所以事情是这样的。我尝试编辑 bakeconf.xml 文件，使其具有我的特殊文件名。所以你在上面的文件夹中看到的文件，但它不起作用，我不知道如何让 bake 刷新并使用更新的配置文件，所以我只是在 bakeconf.xml 文件中查看它:

```
<attribute name="dependency_test" value="(_gv.so or _graphviz.so or _graphviz.x86_64-linux-gnu.so or _graphviz.cpython-38-x86_64-linux-gnu.so or _graphviz.cpython-39-x86_64-linux-gnu.so)"/>
```

看，它在找一个有这些名字的文件。所以我决定用它正在寻找的指向我的文件的名字做一个符号链接。所以文件中包含 pygraphviz)

```
cd /usr/lib/python3/dist-packages/pygraphviz/
sudo ln -s _graphviz.cpython-36m-x86_64-linux-gnu.so _graphviz.so
```

现在，当我在 pygraphviz 文件夹中使用“ls -halp”时，我会看到:

```
drwxr-xr-x   4 root root 4.0K Jan  6 12:51 ./
drwxr-xr-x 151 root root  12K Jan  6 12:16 ../
-rw-r--r--   1 root root  58K Nov  6  2016 agraph.py
-rw-r--r--   1 root root  67K Jun 27  2019 _graphviz.cpython-36m-x86_64-linux-gnu.so
-rw-r--r--   1 root root 7.9K Nov  6  2016 graphviz.i
-rw-r--r--   1 root root 8.7K Nov  6  2016 graphviz.py
**lrwxrwxrwx   1 root root   41 Jan  6 12:51 _graphviz.so -> _graphviz.cpython-36m-x86_64-linux-gnu.so**
-rw-r--r--   1 root root 187K Nov  6  2016 graphviz_wrap.c
-rw-r--r--   1 root root 1.8K Nov  6  2016 __init__.py
drwxr-xr-x   2 root root 4.0K Jan  5 18:06 __pycache__/
-rw-r--r--   1 root root 4.1K Nov  6  2016 release.py
drwxr-xr-x   3 root root 4.0K Jan  5 18:06 tests/
-rw-r--r--   1 root root  198 Nov  6  2016 version.py
```

请注意，我加粗了符号链接。那个小小的“->”字符表示我的链接指向我想要它指向的文件。

基本上是这样的:

```
link_name -> actual_file_pointed_to
```

好了，现在当你运行 bake.py show 时，你应该看到它找到了 pygraphviz

```
-- System Dependencies --
 > g++ - OK
 > gi-cairo - OK
 > gir-bindings - OK
 > mercurial - OK
 > pygobject - OK
 > pygraphviz - OK
 > python-dev - OK
 > python3-dev - OK
 > python3-setuptools - OK
 > qt - OK
 > setuptools - OK
```

就是这样！！

这实际上是一个 pull 请求，他们在 bake 源代码中修复了这个问题:

[](https://gitlab.com/nsnam/bake/-/merge_requests/8/diffs) [## 修复了 pyviz(！8)合并请求 nsnam / bake

### 署名:帕斯·普拉蒂姆·查特吉·parth27official@gmail.com 感谢@汤姆·亨德森先生指出…

gitlab.com](https://gitlab.com/nsnam/bake/-/merge_requests/8/diffs) 

但是，如果你看，你可以看到它是为 Python 3.8 或什么，也可能我没有这个修复进入的 bake 版本或什么。但是，看看他们的修复和他们是如何做到的，这应该足以说明烘焙基本上在做什么。

即使你的问题和我的不完全一样，我想通过阅读这篇文章，你应该能够编辑 bakeconf.xml 或者使用符号链接来解决你的问题。

如果你不介意花时间，请评论你的解决方案，它可能会帮助其他人解决类似的问题，如果你认为它会帮助某人，请随时链接到相关 Github 问题中的这篇文章。

谢谢，

阿什利·塔普