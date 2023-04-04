# 如何使用 UIDocumentPickerViewController 将文件导入您的应用程序

> 原文：<https://blog.devgenius.io/how-to-import-files-into-your-app-using-uidocumentpickerviewcontroller-772ce693bfa4?source=collection_archive---------3----------------------->

随着 iOS 13 中文件应用的引入，你现在可以访问你存储在 iCloud Drive、Google Drive、Drop box 等中的所有文件。这为文本和图像编辑器等更多基于文档的应用铺平了道路。

![](img/7f5eca073b10296e73fe1c8f0220896b.png)

Maksym Kaharlytskyi 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

对于我正在处理的一段代码，我需要让用户能够从他们的文件中选择 PNG 图像，然后在我的应用程序中操作这些图像。

我使用了[UIDocumentPickerViewController](https://developer.apple.com/documentation/uikit/uidocumentpickerviewcontroller)来启用这个特性。它支持四种处理外部文档的模式:

*   导入(将外部文档复制到您的应用程序中，保持原始文档不变)
*   打开(提供对文档的访问，并且可以就地编辑)
*   导出(将应用程序中的文档导出到您选择的外部目标位置)
*   移动(在应用程序中重新定位文档的外部目标位置)

我只需要为我的应用程序的目标导入模式。

首先创建一个 uidocumetcipickerviewcontroller

**kUTTypePNG** 告诉文档拾取器我只想拾取 PNG 文件。您可以指定多种文件类型，如 jpeg ( **kUTTypeJPEG** )和 pdf(**kuttypedf**)等。

> **注:**在撰写本文时，苹果开发者文档已经列出了当前的全局标识符变量文件将在 iOS 14(将于今年晚些时候正式发布)中被替换。你可以在这里看到新的类型。

扩展您的视图控制器以实现 UIDocumentPickerDelegate 协议:

并且将 PNG 文件导入到我的应用程序中！感谢阅读😊