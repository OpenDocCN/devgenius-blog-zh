# 如何在 SwiftUI 中设置不同选项卡的图标颜色

> 原文：<https://blog.devgenius.io/how-to-set-different-tabitems-icon-colors-in-swiftui-7a9bb748f1a3?source=collection_archive---------3----------------------->

如果你曾经试图改变你的`TabView`中`TabItems`的颜色，你可能会发现这只能通过`TabViews` `accentColor`修改器来实现——它为所有的`TabItems`设置颜色。

当切换到不同的选项卡时，如果您希望每个项目具有不同的颜色，请使用`.onAppear(perform:)`设置强调颜色。

该代码会导致:

![](img/7feca7472e5c7fcf0e779dafc254609e.png)