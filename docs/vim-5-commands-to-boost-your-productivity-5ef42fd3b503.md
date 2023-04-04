# VIM——提高生产力的 5 个命令

> 原文：<https://blog.devgenius.io/vim-5-commands-to-boost-your-productivity-5ef42fd3b503?source=collection_archive---------3----------------------->

![](img/a1eb41e4560e1211e2404fdc28c804f0.png)

## 1.制表符

在 VIM 中，您可以有选项卡。这是一个我很久都不知道的功能，对我很不利！

要创建、关闭和删除选项卡，请使用以下命令:

```
:tabe # Open a new tab
:tabc # Close the current tab
:tabo # Close all but the current tab
```

改变标签页是通过`gt`实现的，转到下一个标签页`gT`是你的朋友。要获得更多高级信息，您可以直接跳转到特定标签，例如:`3gT`将转到第三个标签。

## 2.格式化 JSON

JSON 可以在 VIM 中格式化，注意`jq`安装在操作系统上，这是一个轻量级和灵活的命令行 JSON 处理器。通过选择 JSON 块并运行以下命令:

```
:%!jq .
```

## 3.排序文本

要对 VIM 中的文本进行排序，选择所需的文本，并在所选文本后直接输入以下命令:

`:sort`

默认情况下，排序是按字母顺序的。

## 4.行号

`:set number`命令将显示一个带有行号的 VIM 文件。行号便于引用代码中的特定部分。在使用其他 VIM 命令时，它们也可以提供帮助。

或者，使用此命令`:set nonumber`隐藏行号。

## 5.快速浏览 VIM 文件

您可以使用以下命令浏览 VIM 文件:

*   `SHIFT + L`(低)将光标跳到当前屏幕的底部
*   `SHIFT + M` (mid)将光标跳转到当前屏幕的中央
*   `SHIFT + H`(高)将光标跳转到当前屏幕的顶部

希望你已经学会了 5 个新的 VIM 命令来提高你的生产力和 VIM 知识！