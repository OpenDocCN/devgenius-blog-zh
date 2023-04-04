# 如何在 Linux 中用 ls 命令只列出某些文件扩展名

> 原文：<https://blog.devgenius.io/how-to-list-only-certain-file-extensions-with-the-ls-command-in-linux-3bd0342f2892?source=collection_archive---------8----------------------->

![](img/df65b2208559a45aa521b639ab1fc658.png)

由 [cottonbro](https://www.pexels.com/@cottonbro/) 在[像素](https://www.pexels.com/photo/boy-in-white-t-shirt-sitting-on-chair-in-front-of-computer-4709285/)上拍摄的照片。

`ls`命令是使用 Linux 终端时最基本的命令之一。

如果你不熟悉这个命令，当你想查看一个目录的内容时，无论是子目录还是文件，你都会用到这个命令。在这种情况下，它是您可能更习惯的东西的文本表示，比如文件浏览器或查找器。

当您键入`ls`然后按回车键时，您可能会看到类似这样的内容:

```
$ ls
Applications     Downloads      Pictures
```

假设我们想要进入或者将目录改变到`Downloads`目录。然后，我们将键入`cd`以及我们想要进入的目录(也称为文件夹)。

```
$ cd Downloads
```

如果我们键入`ls`再次查看内容，我们将看到`Downloads`目录中的所有内容。

```
$ ls
recipes.docx    haha.jpg    test.txt    report_card.txt
```

所以现在我们已经做了一些`ls` 101，让我们进入这篇博文想要覆盖的整洁的小技巧:只列出*你想要找到的具有特定文件扩展名的文件。*

换句话说，如果我们只想查看并过滤掉所有其他文件，从而只获得所有以`.txt`结尾的文件，会怎么样？

在我们上面的例子中，我们可以直观地看到`test.txt`和`report_card.txt`是我们想要的文件，但是目录会变得非常大，找到它们不会那么容易和简单。此外，能够让电脑算出来就简单多了。

那么，关键是使用一个**通配符**。

通配符是由单个字符表示的占位符，通常是星号(*)，有时也称为星号。

星号的意思是一个或多个字符可以代替它。这就意味着“这个星星可以是任何字符串”。Like a `*`可以表示“汽车”，就像它可以表示“a”或“fdjkafjhdsfkdsfhadshfdskkjdsfsf”一样。所有这些都符合`*`的定义。

所以让我们回到获取所有以`.txt`结尾的文件——为此，您可以运行以下命令:

```
$ ls *.txt
```

这意味着任何至少包含一个字符的东西，特别是以字符串 `***.txt**` **结尾的**，都将作为命令**的结果返回。**

因此，您会得到:

```
$ ls *.txt
test.txt    report_card.txt
```

有道理吗？这里，让我们试试其他的文件扩展名。

让我们开始获取所有属于`.docx`文件的文件:

```
$ ls *.docx
recipes.docx
```

很好。现在让我们对所有的`.jpg`文件做同样的操作:

```
$ ls *.jpg
haha.jpg
```

太棒了。我们现在以一种更先进的方式利用`ls`来过滤文件扩展名！

如果你觉得这篇文章很有帮助或者只是喜欢阅读它，可以考虑[注册成为](https://tremaineeto.medium.com/membership)的媒体会员。每月 5 美元，你可以无限制地阅读媒体上关于软件、技术等主题的报道。如果你用我的链接注册，我会得到一点佣金。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入 Medium—Tremaine Eto

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)