# [程序员]工具注释

> 原文：<https://blog.devgenius.io/programmer-notes-for-tools-2a1256f245a3?source=collection_archive---------27----------------------->

私人使用的笔记。如果对你有帮助，我会很高兴。

![](img/4a6e60eec2df8d046634e399e4013ccf.png)

[Zan](https://unsplash.com/@zanilic?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 编者ˌ编辑

我使用了一个 [**vscode**](https://code.visualstudio.com/) 和 [**vim** 扩展](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim)的(不寻常)组合作为 **Windows** 平台下的编辑环境。这对我来说是最好的组合，因为:

1.  vscode 速度很快，对于许多任务来说“很有效”。(不敢相信我为了微软的东西说了这些。)
2.  vim 使编辑的生产力**数量级**优于你所见即所得的编辑。为我工作了将近 20 年。

但是，像任何环境/工具一样，我仍然会遇到麻烦。当我发现值得注意的解决方案时，我就把它记录在这里。

## 蛇案到骆驼案

[Dart](https://dart.dev/) 约定使用小写 camel case(例如 thisIsLowerCamelCase)表示非常量变量。如何有效地将蛇案(如 this_is_snake_case)转化为下驼案**？**

**在 vscode + vim 下，这变得有点棘手:**

1.  **选择菜单`Edit > Find` ( `Ctrl + F`映射为 vim 下的“向下翻页”。)**
2.  **`Alt + R`启用正则表达式。**
3.  **输入`_[a+z]`作为搜索模式。**
4.  **`Alt + Enter`到[选择所有出现的查找匹配](https://code.visualstudio.com/docs/getstarted/keybindings)。**
5.  **【选项】`Alt + Left Mouse Click`删除不想要的匹配。(例如注释中的文本。)**
6.  **将光标移动到下划线处(向左移动两次)。**
7.  **`x`删除下划线。**
8.  **`~` (Shift +`)到[将机箱从下往上切换](https://vim.fandom.com/wiki/Switching_case_of_characters)。**

**就是这样。如果您的情况不同，通常您可以通过在步骤 3 中更改正则表达式来修复它。**

**PS:第四步是解决这个问题的关键，感谢[乌戈斯塔](https://gist.github.com/Ugotsta)的励志[注](https://gist.github.com/Ugotsta/2a271f6440f1ac7e028df55e94035e40)。**

**![](img/7ea19afaf2e4d95283314f8baa302a39.png)**

**中建议放‘高质量图像’，所以给你…**