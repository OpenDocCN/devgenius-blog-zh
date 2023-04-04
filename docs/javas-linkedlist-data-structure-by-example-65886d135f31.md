# Java 的链表数据结构举例

> 原文：<https://blog.devgenius.io/javas-linkedlist-data-structure-by-example-65886d135f31?source=collection_archive---------2----------------------->

![](img/0f6389c91b8d213a0844a723ace2503d.png)

了解特定数据结构的一种方法是简单地阅读文档。但是除非你自己树立榜样，否则你不会完全理解。在本文中，我们将看看 [LinkedList](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) 类。电话簿似乎是大多数大学教授介绍这种[数据结构](https://en.wikipedia.org/wiki/Data_structure)的首选示例。在我们的电话号码簿计划中，我们希望能够:

1.  添加新记录，
2.  删除当前记录，
3.  更改当前记录的名字，
4.  更改当前记录的姓氏，
5.  更改当前记录的电话号码，
6.  选择当前记录，
7.  显示所有记录和
8.  退出程序。

电话簿将存储名字、姓氏和电话号码。当插入时，我们想要将新记录插入到正确的顺序中，而不需要创建或使用[排序算法](https://www.cs.cmu.edu/~adamchik/15-121/lectures/Sorting%20Algorithms/sorting.html)。您可能有重复的姓名，但没有重复的电话号码。电话号码簿应该按姓氏、名字和电话号码排序。删除记录时，不会选择当前记录。要在删除后选择记录，必须使用选择当前记录选项。除非选择了当前记录，否则您将无法更改名字、姓氏或电话号码。

下面是我的电话号码簿程序。为了帮助理解这个逻辑，从 [*main()*](http://www.cs.princeton.edu/courses/archive/spr96/cs333/java/tutorial/java/anatomy/main.html) 方法开始。它实例化它所在的[类](https://en.wikipedia.org/wiki/Class_(computer_programming))，并调用 *start()* 方法。如果你看一下上面的 *main()* 方法，你会注意到三个[实例变量](http://www.tutorialspoint.com/java/java_variable_types.htm):[*linked list*](https://en.wikipedia.org/wiki/Linked_list)*<Person>persons*， *Scanner input* ，以及 *int currentRecord* 。 *persons* 变量是您在创建 *Person* 对象时存储它们的地方(转到类的底部，您会注意到一个名为 *Person* 的[嵌套类](https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html)，它具有 *firstName、lastName、*和 *phoneNumber* 实例变量)。输入变量的类型是 [*扫描仪*](https://docs.oracle.com/javase/8/docs/api/java/util/Scanner.html) ，这意味着当我们需要时，它会等待用户输入。并且 *currentRecord* 变量保存当前记录。

当您调用 *start()* [方法](http://www.tutorialspoint.com/java/java_methods.htm)时，会显示一个提示，用户可以选择输入一个值。如果他们没有从可用值中选择，将显示一个错误，并要求用户再次输入一个可用值。使用电话目录的常用方法如下:

1.  用户选择添加新记录。

*   提示输入名字。
*   提示输入姓氏。
*   提示输入电话号码。
*   检查以确保电话号码正确。
*   检查以确保该电话号码不在 LinkedList 中。
*   创建一个新的*人物*对象。
*   调用 *getIndexOfInsertion()* 方法来确定在哪里按顺序插入新创建的 *Person* 对象。
*   将*人物*对象以正确的顺序插入*人物*链接列表。
*   显示当前记录。

2.用户选择改变名字

*   提示输入名字。
*   获取当前记录并将其存储在一个 *temp* 变量中。
*   从 LinkedList 中删除当前记录。
*   修改存储在 *temp* 变量中的 *Person* 对象的名字。
*   因为名字的改变可能会改变电话簿中的位置，所以调用了 *getIndexOfInsertion()* 。
*   更新后的*人员*被重新插入*人员*链接列表。
*   显示当前记录。

3.用户选择改变姓氏。

*   采用几乎相同的方法来改变名字。

4.用户选择改变电话号码。

*   改变名字和姓氏的类似方法。
*   唯一的区别是程序必须检查和验证 a)号码是正确的，b)号码不存在于 LinkedList 中。

5.用户可能会在电话目录中添加更多的记录。

6.用户选择显示所有记录。

*   将显示一个可展示的列表。
*   要特别注意确保所有列都正确对齐。
*   我没有包括调整列标题大小的方法，但是如果您愿意，您可以这样做。你只需要得到最长的名和姓，然后就可以了。

7.用户选择删除当前记录。

*   当前的 Person 对象从 persons LinkedList 中删除。

8.用户选择新的当前记录，将新记录添加到电话目录或退出程序。

我们开始吧。创建 phonedir.java 文件，并开始编写一些代码。正如我前面提到的，在我下面的例子中，从 *main()* 方法开始，并从那里开始工作。阅读每个方法的前置条件和后置条件。这将告诉您该方法在使用前所期望的条件以及您可以预期的结果类型。

![](img/52501a5285cdddef01778215c987b707.png)

迪诺·卡希奇目前是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物科技](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读迪诺·卡吉克(以及媒体上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。