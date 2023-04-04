# Python Up 你的代码:模块和包

> 原文：<https://blog.devgenius.io/python-up-your-code-modules-and-packages-13b05ae04226?source=collection_archive---------11----------------------->

## 试图澄清“模块 vs 包”的困境

![](img/bbcd12355bc14e6b91821096c81b2781.png)

Jr Korpa 在 [Unsplash](https://unsplash.com/s/photos/abstract?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

本文旨在阐明并正确界定这两个概念，因为对于什么使模块成为模块，什么构成 Python 中的实际包，似乎仍然存在某种程度的混淆。

## 那么，Python 中的模块是什么？

最简单的回答是:这是一个文件。仅此而已。一旦我们清除了这个文件，我们应该注意一些使这个文件成为真正的 Python 模块的特征:

*   它有通常的`py`扩展名(例如，`my_module.py`是模块名的一个很好的例子)；
*   它的名字(这更像是一种约定)应该很短，所有的小写字母和下划线都是允许的，如果它们能提高可读性的话。更多关于这个[这里](https://peps.python.org/pep-0008/#package-and-module-names)。其他类型的字符，比如连字符，或者几乎所有不能用于变量名的字符，在这里也是严格禁止的；
*   它包含 Python 定义(如类定义、函数定义等等)和/或语句。

让我们构建这样一个模块，然后看看如何很好地利用它:

*   ***my _ module . py:***

```
**def** my_func():
    **print**("Hello from my_func!") **def** my_other_func():
    **print**("Hello from my_other_func!")
```

现在，为了使用它，我们需要把它`import`融入到我们的工作中。因此，假设我们正在处理某个文件，该文件应该使用我们刚刚构建的模块，并且该文件名为`hello_world.py`，下面是我们必须要做的事情:

*   ***hello _ world . py:***

```
*# import the module into our global namespace* **import** my_module *# run the functions contained in that module* my_module.**my_func**()
my_module.**my_other_func**()Output:
Hello from my_func!
Hello from my_other_func!
```

我们刚刚做的是**将`my_module` Python 模块**导入到我们的`hello_world.py`全局名称空间中(为了验证这一点，只需在 import 语句:`**print**(**globals**())`之后添加这个代码，它将向您显示全局名称空间中的所有标识符，包括`my_module`)。通过这个`my_module`标识符，我们现在可以访问包含在那个模块中的两个函数。

请注意，正如我们刚才所做的，导入整个模块并不意味着函数本身是**导入的**。相反，它们可以通过全局名称空间中新添加的模块进行访问。

我们可以这样做的另一种方法如下:

```
*# only import the function(s) that we need from the module* **from** my_module **import** my_func *# run the function* my_func()Output:
Hello from my_func!
```

使用这种替代形式的导入，我们没有导入整个模块，而是只导入了我们需要使用的函数。因此，我们可以只通过它的名字来调用这个函数，不需要任何模块前缀，不像我们的第一个例子，这个函数只能通过我们导入的模块来访问。

## __name__ 和“__main__”的事情

在 Python 脚本中，我们并不经常遇到这行代码:

```
**if** __name__ == "__main__":
 *# code*
```

但是这到底有什么用呢？

首先分享一些背景/知识。执行文件时，Python 为这个模块定义了一个变量。其标识符为`__name__`，其值通常为`__main__`。但是如果这个模块没有被直接运行，而是被导入到另一个模块中，那么这个`__name__`变量将获得导入模块的名称。因此，`if __name__ == "__main__"`条件验证了这一点，并允许模块的不同行为，这取决于它是作为主程序运行，还是仅仅导入到另一个作为主程序运行的模块中。

*   ***my_module.py***

```
**if** __name__ == "__main__":
    **print** (f"My name is {__name__} and I run as a main program")
**else**:
    **print** (f"My name is {__name__} and I am being imported")
```

运行该程序(作为主程序)会产生以下结果:

```
My name is __main__ and I run as a main program
```

但是运行一个不同的导入`my_module`的模块:

*   ***my _ other _ module . py***

```
**import** my_module
*# or "****from*** *my_module* ***import*** *my_func", we'd have the same result*
```

会给我们带来以下结果:

```
My name is my_module and I am being imported
```

它通常在没有我前面提到的`else`子句的情况下使用，它最广泛的用途是防止代码在导入时被执行。通常，当我们打算将模块作为主程序运行时，我们只希望执行模块的代码:

```
**def** my_func()
    print("Hello")*# only execute when being run as a main script/module* **if** __name__ == "__main__":
    my_func()
```

## 好吧，那包裹是什么？

Python 中的包——非常简单地说——是一个目录。有一些特殊的特征:

*   它应该包含一个(或多个)Python 模块文件；
*   它还必须包含一个名为`__init__.py`的特殊 Python 文件。这个文件在 Python 将我们的目录识别为一个包的过程中起着重要的作用。简而言之，`__init__.py`很可能是空的。它的存在确保 Python 将我们的目录视为一个包。

`__init__.py`还有另一个用途，那就是定义从模块中导入什么资源。这些资源将可以使用包标识符直接访问。举个例子吧。

假设我们有一个名为`my_package`的目录，包含两个文件:`__init__.py`和一个模块:`my_module.py`。这些文件的内容如下:

*   ***my_module.py*** :

```
**def** my_func():
    **print**("Hello from my_func!")**def** my_other_func():
    **print**("Hello from my_other_func!")
```

*   ***__init__。py*** :

```
*# only make "my_func" available from my module* **from** .my_module **import** my_func
```

现在，假设我们有一个脚本文件`my_script.py`，与我们的包(目录)在同一层:

```
*# import the package* **import** my_package *# only my_func is now available to import from the package,
# as specified in the package's __init__.py file*
my_package.**my_func**()*# as such, this will throw an exception* my_package.**my_other_func**()Output:
Hello from my_func!
Traceback (most recent call last):
  ...
    my_package.my_other_func()
AttributeError: module 'my_package' has no attribute 'my_other_func'
```

我们得到的那个异常已经被抛出，因为我们的`__init__.py`文件只从`my_module`导入了`my_func`。当然，这都可以通过从包中显式导入模块并使用它所提供的所有功能来绕过:

```
*# import the module directly* **import** my_package.my_module*# now we have access to all its functions*
my_package.my_module.**my_func**()
my_package.my_module.**my_other_func**()Output:
Hello from my_func!
Hello from my_other_func!
```

虽然这是一次非常整洁的模块与包之旅，但我对这两个概念的概述已经接近尾声。确保你也查看了关于这个主题的官方文档，我相信你会在那里找到很多其他的好东西(在知识方面)。

直到[下一个](https://medium.com/@deck451/python-up-your-code-the-diamond-problem-fb3748ad4ad8)时间！祝编码愉快，保持安全！

*Deck 是软件工程师、导师、作家，有时甚至是老师。他拥有 12 年以上的软件工程经验，现在是 Python 编程语言的真正倡导者，同时他的热情是帮助人们提高他们的 Python(以及一般的编程)技能。你可以在*[*【Linkedin】*](https://www.linkedin.com/in/deck451/)*[*脸书*](https://www.facebook.com/deck451/)*[*推特*](https://twitter.com/Deck45100) *，以及* [*不和*](https://discord.com) *: Deck451#6188，以及跟随他在这里写作的* [*中*](https://medium.com/@deck451)**

***更多内容请看*[***plain English . io***](https://plainenglish.io/)*。报名参加我们的* [***免费周报***](http://newsletter.plainenglish.io/) *。关注我们关于*[***Twitter***](https://twitter.com/inPlainEngHQ)*和*[***LinkedIn***](https://www.linkedin.com/company/inplainenglish/)*。查看我们的* [***社区不和谐***](https://discord.gg/GtDtUAvyhW) *加入我们的* [***人才集体***](https://inplainenglish.pallet.com/talent/welcome) *。***