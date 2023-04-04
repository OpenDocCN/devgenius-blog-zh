# flutter:Starter # 2 的小部件

> 原文：<https://blog.devgenius.io/flutter-widgets-for-starter-2-40649c9885b5?source=collection_archive---------36----------------------->

![](img/f19cf2181b44fdd5a976fa16b9c4b6ae.png)

克里斯托弗·高尔摄于 Unsplash

所以，我们又回到了我们的困惑:初学者的小部件！现在，我们在这里进行下一部分！只是回顾一下[Flutter:Starter Part 1](https://medium.com/dev-genius/flutter-widgets-for-starter-1-6aa0860039b6)的小部件。

*   Flutter 小部件简介
*   文本小部件
*   容器
*   圆柱
*   排
*   扩大

如果你错过了这篇文章，这里有给你的[链接](https://medium.com/dev-genius/flutter-widgets-for-starter-1-6aa0860039b6)！💙💙💙

让我们开始第 2 部分吧！你们兴奋吗？！

GIF by Hannah by giphy

# 1)按钮部件

在 Flutter 中，按钮部件是用户的普遍需求。Flutter 也有过几种类型的按钮，像 ***平面按钮，上升按钮，轮廓按钮，还有更多按钮*** ！

在本次会议中，我们将解决很多按钮问题！

1.  **平面按钮**
2.  **凸起按钮**
3.  **轮廓按钮**
4.  **图标按钮**
5.  **浮动动作按钮**

# 1.1)扁平按钮

在颤振文件中，有一个材质设计"**平钮**"。一个**平面按钮**是一个显示在(零标高)材质小部件上的文本标签，它通过填充颜色对触摸做出反应。

***阅读更多:*** [***平按钮***](https://api.flutter.dev/flutter/material/FlatButton-class.html) ***来自飘起的文档。***

# 1.2)凸起按钮

在 Flutter 文档中，一个材质设计凸起的按钮。凸起的按钮由悬浮在界面上的一块矩形材料组成。

***阅读更多:*** [***凸起按钮***](https://api.flutter.dev/flutter/material/RaisedButton-class.html) ***从飘起的文档。***

# 1.3)轮廓按钮

在 Flutter 文档中，一个中等强调的按钮，用于重要的辅助操作，但不是应用程序中的主要操作。

***阅读更多:*** [***轮廓按钮***](https://api.flutter.dev/flutter/material/OutlineButton-class.html) ***来自扑文档。***

# 1.4)图标按钮

在 Flutter 文档中，图标按钮是打印在材质小部件上的图片，它通过填充颜色(墨水)对触摸做出反应。

对于图标，一个图形图标小部件，用一个在[图标数据](https://api.flutter.dev/flutter/widgets/IconData-class.html)中描述的字体绘制，例如在[图标](https://api.flutter.dev/flutter/material/Icons-class.html)中材质的预定义[图标数据](https://api.flutter.dev/flutter/widgets/IconData-class.html)。

你可以在这里阅读更多关于图标[](https://api.flutter.dev/flutter/widgets/Icon-class.html)*。*

****阅读更多:*** [***图标按钮***](https://api.flutter.dev/flutter/material/IconButton-class.html) ***来自飘起的文档。****

# *1.5)浮动操作按钮*

*在 Flutter 文档中，浮动动作按钮是一个圆形的图标按钮，它悬停在内容上以提升应用程序中的主要动作。浮动动作按钮最常用于 Scaffold.floatingActionButton 字段。*

****阅读更多:*** [***浮动动作按钮***](https://api.flutter.dev/flutter/material/FloatingActionButton-class.html) ***来自飘动文档。****

*按钮就这么多了！那么，让我们继续进行文本输入。*

# *2)文本字段*

*在 Flutter 中，文本字段允许我们使用来获取输入。它们用于在我们的应用程序中构建消息、搜索或其他输入。*

*在 TextField 小部件中，我们还可以添加装饰，如放置轮廓或内嵌边框、半径，以及更多可以用样式装饰的东西！*

****阅读更多:***[***TextField***](https://api.flutter.dev/flutter/material/TextField-class.html)***来自扑文档。****

*除了文本字段小部件。我们将在本文中添加这些主题。*

*   *onChanged*
*   *已提交*
*   *控制器*
*   *TextFormField*

# *2.1) TextField: onChanged*

*在 Flutter 文档中，当用户开始更改文本字段的值时:当他们插入或删除文本时。*

****阅读更多:***[***onChanged***](https://api.flutter.dev/flutter/material/TextField/onChanged.html)***来自扑文档。****

# *2.2)文本字段:onSubmitted*

*在 Flutter 文档中，当用户表示他们已经完成了对字段中文本的编辑时。*

****阅读更多:*** [***提交***](https://api.flutter.dev/flutter/material/TextField/onSubmitted.html) ***来自飘起的文档。****

# *2.3)文本框:控制器*

*在 Flutter 文档中，控制正在编辑的文本。*

****阅读更多:*** [***控制器***](https://api.flutter.dev/flutter/material/TextField/controller.html) ***来自颤振文档。****

# *2.4)文本表单域*

*在 Flutter 文档中，包含文本字段的表单字段。这是一个方便的小部件，它将一个 TextField 小部件包装在一个 FormField 中。*

*不需要前祖先。*

*该表单使一次保存、重置或验证多个字段变得更加容易。*

*从代码中可以看出，我们有 onSaved 和 validator。*

*   *onSaved —通过从 FormState 保存表单时，用最终值调用的可选方法。*
*   *验证器—验证输入的可选方法。如果输入无效，返回要显示的错误字符串，否则返回 null。*

*那么，今天的会议到此结束！我希望你今天能学到新的东西！*

*此外，我要感谢扑了最好的文件！你可以通过这个[链接](https://flutter.dev/docs)提前了解更多信息。*

*来自吉菲*