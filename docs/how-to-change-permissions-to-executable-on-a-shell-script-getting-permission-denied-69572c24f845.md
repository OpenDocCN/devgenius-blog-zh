# 如何在获得“权限被拒绝”的 shell 脚本上更改可执行文件的权限

> 原文：<https://blog.devgenius.io/how-to-change-permissions-to-executable-on-a-shell-script-getting-permission-denied-69572c24f845?source=collection_archive---------4----------------------->

![](img/8150cc65f835797d37ffd8774eb71900.png)

由 [Markus Spiske 在 Unsplash 上拍摄的照片](https://unsplash.com/photos/LVieFk4bJSo)

# 错误

如果您在这里，那么当您尝试运行一个 shell 脚本时，很有可能在 Linux 中遇到以下错误:

```
.your-script.sh: Permission denied
```

这意味着您试图运行的 shell 脚本没有正确的权限，在本例中是可执行权限。

# 修复

为了授予权限，您可以使用`chmod`命令和`-x`标志:

```
chmod +x your-script.sh
```

在这种情况下，文件名是`blah.sh`，但是您可能想要用您的文件名来替换它。

现在，尝试再次运行您的脚本，您应该能够克服错误:

```
./your-script.sh
```

# 关于修复的更多背景

之所以必须使用`chmod`是因为在 Linux 中该命令修改文件权限。

`chmod` [手册页](https://linuxcommand.org/lc3_man_pages/chmod1.html)对此进行了如下描述:

> chmod 根据 mode，
> 改变每个给定文件的文件模式位，mode 可以是要进行的改变的符号表示，
> 或是代表新模式位的位模式的八进制数
> 。

老实说，这是相当低级的，但是让我尝试在一个更高的层次上描述`chmod`。

`chmod`允许您控制文件的读取、写入和执行能力。`x`标志专门用于执行权限；它前面的`+`意味着除了已经在该文件上的任何其他权限之外，我们还授予这个权限。

综上所述，用简单的英语来说，该命令只是说，“通过授予它执行权限来更改`your-script.sh`的权限。”

一点也不差，对吧？

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)