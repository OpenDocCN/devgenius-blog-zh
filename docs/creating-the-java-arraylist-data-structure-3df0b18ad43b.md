# 创建 Java 数组列表数据结构

> 原文：<https://blog.devgenius.io/creating-the-java-arraylist-data-structure-3df0b18ad43b?source=collection_archive---------1----------------------->

![](img/804ca006a29d640afddafab8f1262af1.png)

Java 中的某些数据结构可以由你来创建(是的，你)。在这个例子中，我们将继续创建一个 ArrayList 数据结构，它包含内置 ArrayList 类的一些方法。

我们将创建两个构造函数:

*   创建默认大小为 10 的 ArrayList 的默认构造函数。
*   允许向数组传递初始大小的构造函数。

我们还将创建一些方法:

*   **void add(对象 x)；**一种允许你在数组列表末尾放置一个对象的方法。
*   **void add(int index，Object x)；**一种允许你在给定位置放置一个值的方法。
*   **Object get(int index):** 允许你从一个给定的位置获取一个数组列表的值。
*   **int size()；**允许你获取数组列表中当前元素的数量。
*   **布尔型 isEmpty()；**测试数组列表是否为空。
*   **布尔 isIn(对象 x)；**查看数组列表中是否存在特定对象的方法。
*   **int find(对象 x)；**返回从位置 0 开始的对象第一次出现的位置。
*   **void remove(对象 x)；**从位置 0 开始删除对象的第一个匹配项。

我鼓励你不要去看 ArrayList 内置类。看你自己能不能搞清楚。唯一的其他限制是将对象存储在数组数据字段中。创建一个测试类来测试 ArrayList 类。将数组列表类命名为 ArrayList.java，这样它会覆盖内置的类。

阅读每种方法上面的注释。有一个前提条件，规定了在调用方法之前需要遵守的要求。例如，如果该方法需要向其传递一个整数数组，则需要准备好一个整数数组。方法被调用后，就有了后置条件。这解释了该方法的预期输出是什么，以及该方法实现该输出所采取的步骤。

我们将从数组列表类开始。上面概述的方法将被添加到这个类中。

如果你不去测试它，代码有什么用？我们将创建一个驱动程序类来测试数组列表代码。

这就是他们的全部。有了基本的 Java 技能，您也可以创建自己的数组列表数据结构。

![](img/58f16333e2b15ffd07411a1154ad49d0.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)