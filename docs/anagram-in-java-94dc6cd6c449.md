# Java 中的变位词

> 原文：<https://blog.devgenius.io/anagram-in-java-94dc6cd6c449?source=collection_archive---------15----------------------->

![](img/3eea0902b4890ddab533399d6ec85845.png)

我简单地浏览了一些计算机科学课程的问题，发现其中一个问题是这样的:

*用 Java 创建一个* [*变位词*](https://en.wikipedia.org/wiki/Anagram) *程序，该程序读取文本文件并计算单词的变位词。如果单词是彼此的变位词，将它们放在同一行；如果不是，就在新的一行上打印。字谜程序应该创建一个新的文本文件。唯一可以使用的数据结构是数组。您必须创建自己的排序算法。*

这是我的解决方案。请务必阅读每个方法的前置条件和后置条件，以便更好地了解该方法需要什么以及它将产生什么输出。从 *main()* 方法开始。

![](img/f2b19349387c42faf86ec922614f3cad.png)

迪诺·卡希奇目前是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物科技](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和[艾阿尔法](https://www.exalpha.com/)的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。