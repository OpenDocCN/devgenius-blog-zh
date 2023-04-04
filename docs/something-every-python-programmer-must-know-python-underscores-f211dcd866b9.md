# 每个 Python 程序员都必须知道的东西，下划线

> 原文：<https://blog.devgenius.io/something-every-python-programmer-must-know-python-underscores-f211dcd866b9?source=collection_archive---------1----------------------->

![](img/b24d1d0142564d2ab2be7bae117b6eca.png)

Python 有如此多令人敬畏但不为人知的特性，即使知道其中的一些也能让你进入高级程序员的行列。其中一个是**下划线**&邓德斯。

在这篇文章中，我将讨论 Python 中所有五种可用的下划线模式，以及它们如何影响 Python 程序的行为。

单下划线和双下划线在 Python 变量和方法名称中有意义。其中一些含义仅仅是约定俗成的，旨在给程序员一个提示——还有一些是由 Python 解释器强制执行的。

**1。单前导下划线:_var**

Python 不像 Java 那样对“私有”和“公共”变量有很强的区分。因此，为了表明变量是供内部使用的，在变量名前使用了一个前导下划线(前缀)。

```
Class Pubber:
  def __init__(self):
    self.name = 'Bond, James'
    self._age = 33 #private variable, shhh...
```

如果在变量名中使用了前导下划线，Python 解释器通常不会强制使用，只是给程序员一个提示。这就像是向其他程序员传达——“嘿，这并不意味着要在这个类的接口之外使用。最好别管它！”

前导下划线可以用于定义私有函数或方法名。

```
# pub_shubs.py def _menu_tonight(): #private function
    something_special
```

如果作为通配符导入，模块中的私有函数将不可访问。别担心，常规进口会起作用的。

```
from pub_shubs import * #wildcard import
_menu_tonight() #NameError: "name '_internal_func' is not defined"import pub_shubs #regular import
pub_shubs._menu_tonight() #No Error
```

**2。单个尾随下划线:var_**

如果你需要用一个关键词作为名字呢？

在变量、函数或类名后面附加一个下划线(后缀),就可以了！它避免了与 Python 关键字的命名冲突。

```
Class class: #SyntaxError: "invalid syntax"Class class_ #No problem
```

**3。领先邓德斯:__var**

邓德斯的意思是双下划线。

Python 中可以使用双下划线前缀来避免子类中的命名冲突。

```
Class Pubber:
  def __init__(self):
    self.name = 'Bond, James'
    self._age = 33
    self.__address = 'Mars' #double underscore prefix
```

让我们使用内置的 dir()函数来看看这个对象的属性:

```
>>> guest = Pubber()
>>> dir(guest)
['_Pubber__address', '__class__', '__delattr__', '__dict__',
'__dir__', '__doc__', '__eq__', '__format__', '__ge__',
'__getattribute__', '__gt__', '__hash__', '__init__',
'__le__', '__lt__', '__module__', '__ne__', '__new__',
'__reduce__', '__reduce_ex__', '__repr__',
'__setattr__', '__sizeof__', '__str__',
'__subclasshook__', '__weakref__', '_age', 'name']
```

你注意到目录列表中的第一项了吗？-> ' _ publiber _ _ address '

这叫名芒令。Python 解释器更改了变量的名称，使得在扩展类时更难产生冲突。它将其更改为:_ClassName__VariableName

现在让我们看看这个变量的行为。

```
>>> guest.name
'Bond, James'
>>> guest.__address
AttributeError:
"'Pubber' object has no attribute '__address'"
```

不要担心，您仍然可以访问它:

```
>>> guest._Pubber__address
'Mars'
```

名称篡改也适用于方法名吗？确实如此，试试看！

现在，这会让你大吃一惊:

```
_Pubber__address = 'Mars' #global variable
Class Pubber:
   def address(self):
       return __address>>> Pubber.address()
'Mars'
```

呼呼！_Pubber__address 被声明为一个全局变量，但是当在类的上下文中声明时，我可以直接引用它。怎么会？因为名字乱写。

**4。前导和尾随邓德尔:__var__**

有前导和尾随双下划线的名字在语言中有特殊用途，有时被称为“特殊方法”或“神奇方法”。

Python 中有将近 100 个内置的 Dunders，比如用于对象构造函数的 __init__ 或使对象可调用的 __call__ 等。

Python 并不禁止您在自己的程序中使用以双下划线开头和结尾的名称，但是建议您避免使用，因为这可能会与 Python 语言的未来变化相冲突。

**5。单下划线:_**

最后，但同样重要的是，使用一个单独的下划线作为名称来表示变量是临时的或无关紧要的。

```
for _ in range(5):
  do_something
```

还可以在解包表达式中使用单下划线作为“无关”变量来忽略特定值。

```
>>> beer = ('light', 'bitter', 70, 153)
>>> color, _, _, calories = beer
```

有时，在将一个元组解包成单独的变量时，您只对少数特定字段的值感兴趣。所以，不感兴趣的字段可以用' _ ':。

```
>>> color
'light'
>>> calories
'153'
>>> _
70
```

除了用作临时变量之外，“_”还是大多数 Python [REPLs](https://pythonprogramminglanguage.com/repl/) (Python 交互式 Shell)中的一个特殊变量，表示解释器评估的最后一个表达式的结果。

我希望你现在会更加喜欢这种不可思议的编程语言，并充分利用它。

*快乐编码，热爱 Python！*