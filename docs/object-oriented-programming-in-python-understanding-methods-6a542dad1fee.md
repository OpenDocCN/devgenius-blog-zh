# Python 中的面向对象编程——理解方法

> 原文：<https://blog.devgenius.io/object-oriented-programming-in-python-understanding-methods-6a542dad1fee?source=collection_archive---------11----------------------->

## 理解 Python 类中不同类型的方法以及如何使用它们

![](img/ebbe0716ced053ecc7310f4e845dc75b.png)

Todd Quackenbush 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在上一篇文章中，我讨论了关于*变量*——一个*类*的两个基本构件之一。在这篇文章中，我们将讨论另一个构建模块— *方法*。如果你需要复习一下*变量*，你可以看看我之前的帖子——[Python 中的面向对象编程——理解变量](https://curious-joe.net/post/2202-03-16-oop-python-variables-in-class/oop-python-inheritance-subclass/)。

# 方法入门

> *方法*是嵌入在类*中的函数。*

*方法*必须至少有一个参数。没有所谓的无参数*方法*。通常情况下，`self`被用作不需要任何自定义参数的*方法*中的参数。`self`是指*对象*，为其调用*方法*，并使*对象的* *变量*和*方法*可供该方法使用。使用`self`这个词是一种通常被遵循的规范。所以即使`self`可以用其他任何词代替，这样做也会增加不必要的惊喜。

# 情况

假设我们成功交付了第一个简单的项目，创建了一个能够添加和删除元素的列表，我们已经有了一个新客户。他想要一个类似的列表，但是有一个稍微严格的要求——他想将元素的类型限制为数字类型。

因为基本要求仍然相同:

*   这是一份名单，
*   应该能够添加和删除元素，
*   并显示整个列表，

我们将重用我们之前创建的类`NumList`，并使用名为`StrictNumList`的*子类*来构建它。在`StrictNumList`中我们可以看到已经定义了两个*方法*:`__check_value()`和`add_value()`。稍后将提供更多相关信息。

```
class NumList:
    def __init__(self, name = ""):
        self.instName = name
        self.__list = [] 

    def add_value(self, val):
        self.__list.append(val)

    def remove_value(self):
        rv = self.__list[-1]
        del self.__list[-1]
        return rv

    def get_list(self):
        return self.__listclass StrictNumList(NumList):
    def __init__(self, name):
        super().__init__(name)

    def __check_value(self, val):
        return str(val).isalpha()

    def add_value(self, val):
        if not self.__check_value(val):
            super().add_value(val)
        else: 
            input_val = input("Insert a number: ")
            val = float(input_val)
            super().add_value(val)sl01 = StrictNumList(name = "Number List B1")
sl01.instName
sl01.add_value(2)
sl01.add_value("abc")Insert a number: 5sl01.get_list()[2, 5.0]
```

# 构造函数——一种特殊的方法

`__init__`或 Python *构造函数*是一种特殊的*方法*，用于初始化*类*。

*   它在每次实例化*类*时运行，并使*类*可用。
*   它必须至少有一个参数— `self`才能使*对象*的属性可用。
*   与其他方法不同，`__init__`不能返回值或者不能在*类*的内部或外部被调用。例如，你不能创建像< `object name` > < `.` > `__init__()`这样的调用方法。

# 方法的类型

像*变量*，*方法*可以是公共的或者私有的，或者更好地称为*部分*私有的(还记得*的名字吗？).*

## 私有方法

*   命名私有*方法*遵循与私有*变量*相同的模式——在方法名前添加两个下划线(`__`)。
*   名称操纵与*类*名称的工作方式与它与*变量*的工作方式相同。

*为什么要用私有方法？*

使用私有*变量*的原因相同。创建这些*方法*的主要原因是为了在其他*方法*中使用，因此它们不需要从*对象*中直接访问。

但是正如我们从上一篇博客中所知道的，名称篡改实际上使它们变得可用。所以使用这个被破坏的名字，我们实际上也可以访问私有的*方法*。

在我们的示例类`StrictNumList`中，我们创建了`__check_value()`作为私有*方法*。因为它的主要用途是在`add_value()`方法内部使用。但是，请检查下面的代码，看看我们如何仍然可以使用损坏的名称访问这个方法！

```
# calling private method - __check_value() using its mangled name 
sl01._StrictNumList__check_value('5.5')False
```

## 公共方法

公共*方法*可以使用点符号直接从*对象*中访问，即<`object name`><`.`><`method name`>。在我们的例子中，`add_value()`是一个公共变量。

🛑但是请注意我们是如何在*超*和*子类*中定义这个方法的。这叫做*方法覆盖*。

> *当你在子类中不同地定义相同的方法时，方法覆盖发生。*

要成功使用方法重写，您需要满足两个条件:

*   这只有在*类*继承上下文中才有可能。或者换句话说，*方法*继承在同一个*类*内部不起作用。
*   *子类*应该与*超类*具有相同的名称和参数数量。

🛑还注意到，来自*超类*的`add_value()`方法是如何在*子类* `SuperNumList`的`add_value()`方法中使用的。

# 下一步是什么

到目前为止，在 Python 系列的 OOP 中，

*   我们已经介绍了*类*和它们的基本构件，
*   我们已经学习了*继承*及其使用*子类*的应用，
*   在这篇文章中，我们详细介绍了*变量*和*方法*。

在下一篇文章中，我们将尝试了解 Python 异常的内幕。通过这样做，我们将看到学习这些主题将如何派上用场！编码快乐！

**阅读 Python 中 OOP 系列的其他文章:**

[](https://towardsdatascience.com/object-oriented-programming-in-python-understanding-variable-e451cf581368) [## Python 中的面向对象编程——理解变量

### 理解 Python 类中不同类型的变量以及如何使用它们。

towardsdatascience.com](https://towardsdatascience.com/object-oriented-programming-in-python-understanding-variable-e451cf581368) [](https://towardsdatascience.com/object-oriented-programming-in-python-inheritance-and-subclass-9c62ad027278) [## Python 中的面向对象编程——继承和子类

### 理解继承的基本概念，并通过创建子类来应用它们。

towardsdatascience.com](https://towardsdatascience.com/object-oriented-programming-in-python-inheritance-and-subclass-9c62ad027278) [](https://towardsdatascience.com/oop-in-python-understanding-a-class-bcc088e595c6) [## Python 中的 OOP 理解一个类

### 理解 Python 类的基本组件。

towardsdatascience.com](https://towardsdatascience.com/oop-in-python-understanding-a-class-bcc088e595c6) [](https://towardsdatascience.com/object-oriented-programming-in-python-what-and-why-d966e9e0fd03) [## Python 中的面向对象编程——什么和为什么？

### 学习 Python 中的面向对象编程。

towardsdatascience.com](https://towardsdatascience.com/object-oriented-programming-in-python-what-and-why-d966e9e0fd03)