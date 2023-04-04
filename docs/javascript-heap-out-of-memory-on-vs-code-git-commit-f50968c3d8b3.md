# JavaScript 堆内存不足—在 VS 代码/ Git 提交时

> 原文：<https://blog.devgenius.io/javascript-heap-out-of-memory-on-vs-code-git-commit-f50968c3d8b3?source=collection_archive---------3----------------------->

![](img/df1e17843f96d23a72dce7726651ffa3.png)

在使用 VS 代码 git commit 开发 VS 代码苹果 M1 芯片的新项目时，我开始出现以下错误。

> 致命错误:无效标记-接近堆限制的压缩分配失败 JavaScript 堆内存不足

在这里，我会给你两个步骤来解决这个问题。

1.  在 VS 代码上，打开终端并运行以下命令:

```
export NODE_OPTIONS="--max-old-space-size=8192"
```

2.暂存您的更改，并在同一个终端会话中运行:

```
git commit 
```

将很容易提交您的代码。然后你可以应用 **git push** 。

如果您仍然面临相同的问题，请尝试运行增加大小的命令，如 **10240** 或甚至更高，直到您的计算机可以允许的最大内存。

这种方法不仅适用于苹果 M1 芯片笔记本电脑或 VS 代码，也适用于任何出现这种错误的操作系统。

希望这个错误对你有所帮助。

快乐学习