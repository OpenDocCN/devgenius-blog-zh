# 超时空:git 时间机器

> 原文：<https://blog.devgenius.io/chrono-the-git-time-machine-c52e75b48f5c?source=collection_archive---------2----------------------->

![](img/f2848af267aa3ac8f9e60d46d1a06b86.png)

秒表计时标志

如果你从事编程，你可能听说过/使用过 git，它是一个版本控制系统，人们常说

> “Git 是一台时间机器”

因为如果您愿意，可以回滚到以前的版本。

这听起来很棒，但事实是这样的

# 问题是

想象你在做一个项目，有一个`main`分公司，一个`dev`分公司，你从`dev`分支到你自己的`feature`分公司在那里工作，然后和`dev`合并，到目前为止还不错。

有一天，你正在开发一个特性或者修复一个 bug，这花费了你很长时间，让你很头疼，所以你继续工作，忘记了 git 的存在，然后你解决了这个 bug！不错！

*   很好，它工作了！我很快就会承诺
*   *查看文件*
*   这仍然是混乱的
*   我想我会先清理我的代码并重构
*   *触摸代码*
*   妈的，我把它弄坏了
*   我甚至不能回去，因为我没有承诺
*   啊，是的，git 是一个时间机器

我专门创造了一个工具来解决这个问题，它叫做 **Chrono，**

[https://github.com/hazyuun/Chrono](https://github.com/hazyuun/Chrono)

我会在文末再详细说！

# 可能的解决方案

## 解决方案 0

等待物理学家发现回到过去的方法

## 解决方案 1

在做某件事的时候继续提交，我的意思是无论如何提交，即使代码仍然很乱，你也可以在以后修改你的提交

假设您像这样提交了 5 次

```
$ git log --decorate --graph --branches --oneline
* ??????? (HEAD -> mybranch) Everything is alright
* ??????? Fixed a bug
* ??????? Changed that but code is still messy
* ??????? Changed this but code is messy
* ??????? Added feature
```

其中`???????`是哈希

现在你不想要所有的提交，里面一片混乱，所以你可能想把这 5 个提交“合并”成一个。

您可以通过使用 git rebase 来做到这一点，首先运行

```
$ git rebase -i HEAD~5
```

`-i`表示交互(我们稍后会看到)

`HEAD~5`是您想要编辑的最后一个提交的父级，您也可以像`HEAD~N`一样查看它，其中 N 是您想要编辑的提交的数量。

很好，现在一个编辑器将会打开，你提交的内容在最上面，还有一堆以`#`开始的注释帮助你

```
pick ??????? Added feature
pick ??????? Changed this but code is messy
pick ??????? Changed that but code is still messy
pick ??????? Fixed a bug
pick ??????? Everything is alright
```

现在，您可以将这些`pick`更改为`squash`或者只是`s`来压缩它们，对除第一个提交之外的每个提交都这样做，您只能将一个提交压缩到其父提交中。

现在它应该看起来像这样

```
pick ??????? Added feature
s ??????? Changed this but code is messy
s ??????? Changed that but code is still messy
s ??????? Fixed a bug
s ??????? Everything is alright
```

保存并退出编辑器，另一个编辑器会出现，告诉你制作一个提交消息，这样做，你就完成了！

**但请注意，您仍然可以通过找到 HEAD 使用** `**git reflog**` **的位置并返回到该位置来撤销此操作，因此您的混乱可能仍然存在，有一种方法可以删除仅在** `**git reflog**` **中可见的内容，但那是另一天的事情**

## 解决方案 2

进入 [**超时空**](https://github.com/hazyuun/Chrono) ！

Chrono 是我专门为此制作的工具，它会在每次事件发生时自动提交到 git 存储库中的临时分支中(事件是可定制的)，这样，如果出现任何问题，您都可以回滚到特定的时间点。当你完成后，你可以把所有的临时提交合并成一个。

[https://github.com/hazyuun/Chrono](https://github.com/hazyuun/Chrono)

下面是它的使用方法:

**第一步:创建一个 chrono 分支并签出它**

为了避免弄乱你的存储库，Chrono 只能推送到它自己的分支，所以从为 chrono 创建一个分支开始

```
$ git branch chrono
```

git 结帐到它

```
$ git checkout chrono
```

**步骤 2:启动秒表计时功能**
启动秒表计时功能，使用

```
$ chrono session start
```

从现在开始，每当事件发生时，chrono 将自动提交更改，事件可使用`chrono.yaml`文件定制(详情见下文)

**第三步:挤压合并并删除超时空分支**
完成后，你可以将超时空分支合并(推荐挤压合并)到你原来的分支(姑且称之为 original_branch)

```
$ git checkout original_branch
$ git merge --squash chrono
```

然后，如果一切如预期，您可以提交合并

```
$ git commit -m "Your commit message"
```

并删除超时空分支

```
$ git branch -D chrono
```

配置文件
把一个名为`chrono.yml`的文件放在你的库的根目录下，这里是一个配置文件的例子

```
# Events when to automatically commit
events:
 - periodic:
     period: 5
     files: ["src/", "file.txt"]

 - save:
     files: ["notes.txt"]

# Use files: ["."] if you want all files to be commited
```

*   **周期性**每 N 秒触发一次
*   **保存**触发每次文件保存

如果您想在使用`files: [“.”]`时排除一些文件，只需使用您的常规`.gitignore`文件即可

仅此而已！

祝你白天/晚上愉快！

# 链接

*   超时空储存库:[https://github.com/hazyuun/Chrono](https://github.com/hazyuun/Chrono)