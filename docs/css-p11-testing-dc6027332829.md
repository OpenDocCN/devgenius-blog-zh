# CSS — P11:测试

> 原文：<https://blog.devgenius.io/css-p11-testing-dc6027332829?source=collection_archive---------9----------------------->

![](img/525990545acad776446f7bb05891aa7f.png)

当你使用正确的工具时，测试 CSS 是简单的。我们将介绍的一个工具是 Google Chrome 附带的 Google DevTools。要访问 DevTools，右键单击您的站点，然后单击 Inspect。您将看到一个类似这样的页面。

![](img/64a0d493baac4c3d1977e59b102c6673.png)

左侧包含在该屏幕分辨率下看到的您的网站。右边是 HTML 代码，下面是 CSS 代码。分隔左侧和右侧的边框可以向左或向右拖动，以调整包含网页的面板的大小。当您调整页面大小时，屏幕分辨率将出现在网页面板的右上角。

![](img/c9af3db02d4606d641774ce5dd36003d.png)

为了检查页面上每个元素的 HTML 和 CSS，你可以在鼠标悬停在按钮上时点击“选择页面中的一个元素以检查它”，或者在 Windows 上按`Ctrl + Shift + C`或在 Mac 上按`Command + Shift + C`。

![](img/26050386100bc26ff6c7a7d4d6b44de3.png)

选中后，您可以单击页面中的元素来查看 HTML 标记和 CSS 代码。您还会注意到右上面板突出显示了该元素，并且在右下角可以看到与该元素相关的 CSS 代码。

![](img/905c30bc0f1b7c749dc07171784fe571.png)

如果您使用选择工具将鼠标悬停在该元素上，将会出现一个框，显示该元素的详细信息。它显示诸如文本颜色、字体样式、背景颜色、边距和填充等信息。

![](img/83833b1a1a3f1cddc816481577b06b8b.png)

您可能已经注意到元素周围出现了不同的颜色。如果你看屏幕右边的盒子模型，你会看到这些颜色是如何关联的。蓝色代表内容。盒子模型声明内容区域是`174px`宽和`72px`高。绿色显示了`padding`。根据盒子型号，内容区周围有一个`10px` `padding`。我们可以通过查看 CSS 面板中 div 属性的 padding 属性来验证这一点。没有`border`所以颜色不会出现在元素上。有一个`5px` `margin`可以在盒子模型上验证。

您还可以将鼠标悬停在盒子模型的每个部分上，以选择代表该元素的区域。

![](img/af2a6c6ef630e45f29a2459b6290ac98.png)

一旦选择了元素，就可以通过直接在 CSS 面板中修改 CSS 来直接对样式进行更改。这使得实验测试更加容易。一旦您对一切都满意了，就可以将 CSS 代码添加到样式表中了。让我们把`background-color`从浅蓝色改成`yellow`。点击`lightblue`值。它应该突出显示。

![](img/2ef70b1c95cfa0659f03b37394eb45a2.png)

现在只需输入黄色。您会看到更改出现在屏幕的左侧。

![](img/d64e648b8522dbdd8f7f258373078aed.png)

您也可以通过单击 CSS 面板中的元素，向元素添加其他属性，但不要单击任何现有的属性或值。

![](img/150b2f46b925fd6adf3e7a7d4506d03c.png)

让我们添加一个粗体字。如果您查看网页面板，您会注意到字体粗细已经改变。

![](img/c21dc336f3d8241309e0d8e9eeb7eb32.png)

您可能也注意到一些属性有一条线穿过它们。这意味着属性正在 CSS 样式表中的其他地方被覆盖。如果我们想找到`max-width`属性被覆盖的地方，我们可以滚动 CSS 面板并寻找它。

![](img/e3167b7646cfac15bf698ada1a252b3a.png)

在这种情况下，很容易找到。原来由于屏幕分辨率很小，`@media`查询会覆盖`max-width`属性并将其设置为`100%`。

您也可以模拟设备进行进一步测试。要查看设备列表，单击“切换设备工具栏”按钮。您也可以通过按下视窗上的`Ctrl + Shift + M`或苹果电脑上的`Command + Shift + M`来访问它。

![](img/7d7ebbf85f0e89d9a7c16ca2aa56d402.png)

网站面板将会改变，您将会看到设备选择。

![](img/07fb934958913b25b8c1519c15050f4a.png)

如果您让屏幕保持响应状态，您可以通过拖动边缘来调整屏幕的宽度和高度。您也可以选择预定义的分辨率之一，查看您的网站在该设备上的显示方式。让我们从下拉菜单中选择像素 2 开始。您会注意到，现在像素 2 已被选中，屏幕分辨率已更改为`411px x 731px`。

![](img/0d5c21238235d42c10df35bcebd8fd13.png)

您也可以单击手机旋转按钮，查看网站在横向视图中的外观。

![](img/2b5305ab3c1f24df13981936cbd76b73.png)![](img/60680c471dc6272bef0e28e52348f205.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)