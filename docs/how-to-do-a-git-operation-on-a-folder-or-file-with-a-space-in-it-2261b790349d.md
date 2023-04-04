# 如何对包含空格的文件夹或文件进行 Git 操作

> 原文：<https://blog.devgenius.io/how-to-do-a-git-operation-on-a-folder-or-file-with-a-space-in-it-2261b790349d?source=collection_archive---------2----------------------->

![](img/0b6864ea7ac900a528eb9050094c1bc0.png)

由 [Roman Synkevych](https://unsplash.com/@synkevych?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/git?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

最近，我不得不在我的终端中运行 Git 命令，但是我的努力暂时受阻。

原因？我试图反对的文件夹中有一个空格！

在这里，让我解释一下:比如说，这个文件夹被命名为`Members 2021`。

我想在它上面做一个`git rm`，所以我试着从我的命令行运行它:

```
git rm Members 2021
```

然而 Git 抱怨了！不难看出为什么，因为看起来它应该尝试移除`Members`和`2021`只是一些遗留下来的尴尬争论。

所以我在想，我在这里能做什么？用空格代替一个`-`？还是其他逃脱的分隔符？也许甚至一个`%20`或类似的东西会被 URL 编码？

如果你在这里，你很可能来到了一个类似的十字路口，我在这里告诉你，你需要做的是:

**用引号将文件夹或文件中的空格括起来！**

在 Mac 和 Linux 上，你可以添加单引号，但是在 Windows 上你必须使用双引号。

所以基本上，当我再次运行上面的命令时，我做到了:

```
git rm 'Members 2021'
```

…一切都按预期运行！

这适用于所有 git 命令:例如，包括了`add`和`diff`。

所以不要担心:只要你学会了这个快速技巧，一个空格不会毁了你的一天(但希望你能在第一时间找到摆脱这些空格的方法)！

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)