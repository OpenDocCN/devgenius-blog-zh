# 除 console.log()之外的 JavaScript 控制台命令

> 原文：<https://blog.devgenius.io/javascript-console-commands-other-than-console-log-f1973371ee14?source=collection_archive---------7----------------------->

如果你是一个 JavaScript 开发者，你知道你最基本的东西是 console.log()。铝

因此，这里有几个控制台命令，我们可以使用，使我们的调试过程容易得多。

1.  ***console . error()***—将`message`发送到控制台窗口。消息文本为红色*并以错误符号开头。*

![](img/7bfa1b85fe2a722a326e203b856840ab.png)

2.***console . warn()***—将`message`发送到控制台窗口，以警告符号开头。

![](img/5360b24541a72e0ca5609ff425fdea0c.png)

3.***console . table()—***以表格形式显示 JSON / Array，更易于阅读。

![](img/f683373f88418fed45dca9aeee7943a7.png)

4. ***console.group()和 console.groupEnd()***—console . group()和 console . groupend()用于对消息进行分组。

![](img/5c5eb89dc96e3b4bd309c12a2579f9a8.png)

**注意** : *可以用****console . group collapsed()****代替****【console . group()****使其默认以折叠视图出现。*

5.***console . dir()***—在对象可视化器中显示 JSON。使在控制台窗口中检查属性变得更加容易。

![](img/f92803b6c11031bd15c0ffcf90d74c3b.png)

6.***console . assert(expression，message)***—如果`expression`评估为 false，则向控制台窗口发送消息。

![](img/5f7430c4494f5bf8242a0a70b526c347.png)

7.***【console . time(' name ')和 console . time end(' name ')***—用于跟踪它们之间代码执行的时间。它计算`time`和`timeEnd`之间经过的时间，并使用`name`字符串作为前缀将结果(以毫秒为单位)发送到控制台。

![](img/e2d91794b4ec8cddaf3f6cb316901a11.png)

8.***console . clear()***—该命令用于清除控制台消息。只要运行这个命令，你的控制台就会焕然一新。

这就是总结..

谢谢你