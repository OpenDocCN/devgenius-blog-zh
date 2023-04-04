# Python Up 您的代码:枚举

> 原文：<https://blog.devgenius.io/python-up-your-code-enumerations-94eb79641d7e?source=collection_archive---------5----------------------->

## 枚举 Python 模块的简要概述

![](img/09c7f42934f39c3114547de6f8851835.png)

照片由[皮埃特罗·詹](https://unsplash.com/@pietrozj?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/@pietrozj?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 什么是编程中的枚举？

枚举只表示具有关联的唯一常数值的成员的集合。

## 我们为什么要使用枚举呢？

枚举是非常有用的，并且被用于将有限的和固定的状态或实体集合在一起，例如彩虹的颜色，或者一周中的几天，一年中的几个月，http 响应代码，以及可以继续列举的例子。总的来说，它极大地提高了我们代码的可读性和组织性，我们稍后会学到这一点。

## 它是如何工作的？

枚举是存在的，在很多编程语言中都可以找到:C、Java、JavaScript、TypeScript……Python 不会代表一个例外。由于它们的通用性，Python 为我们提供了`enum`模块:

```
*# import the module* **from** enum **import** Enum *# define our enumeration* **class** Weekdays(Enum):
    SUNDAY = 1
    MONDAY = 2
    TUESDAY = 3
    WEDNESDAY = 4
    THURSDAY = 5
    FRIDAY = 6
    SATURDAY = 7
```

这就是我们如何创建自己的枚举。我们所做的是创建了一个自己的类，它继承自`Enum`。我们将我们的类命名为`Weekdays`,以真正反映我们的枚举的全部内容。下一步是通过给每个成员分配一个常数来定义我们的成员。

说到这里，我想简单地与您分享一个细节:请注意定义这些成员的大写样式:因为它们是常量——就我而言，我不认为我们所知道的典型的 7 天工作周会有任何变化——我们的工作日应该使用大写命名样式命名。然而，必须指出的是，仅仅因为我们创建了一个大写的变量，并不意味着我们已经有效地把它变成了一个常量，就像我们在 C ( `const int a = 2;`)中所做的那样。在 Python 中可以很容易地更改它的值:

```
MY_CONSTANT = 5
**print**(MY_CONSTANT)
MY_CONSTANT = 10
**print**(MY_CONSTANT)Output:
5
10
```

那是因为 Python 的**没有**有常量。只有一个变量的**约定**被认为是常量，这表明我们应该通过使用 UPPER_CASE 命名方式来标记我们认为**是常量**的变量。你可以在网上了解更多，这里是一个好的起点。

相反，Python 所拥有的是由对象的可变/不可变属性来表示的(我们是否可以在不改变对象本身的情况下改变对象的值？).

但是，无论如何，回到我们最初的范围，我们已经成功地定义了我们自己的枚举，表示一周中的每一天，每一天都有一个固定的数值分配给它。由于我的*Python 中的常量*离题，我将再次展示代码，以避免您不必要地来回滚动。所以，正如我所说的:

```
*# import the module* **from** enum **import** Enum *# define our enumeration* **class** Weekdays(Enum):
    SUNDAY = 1
    MONDAY = 2
    TUESDAY = 3
    WEDNESDAY = 4
    THURSDAY = 5
    FRIDAY = 6
    SATURDAY = 7
```

现在，让我们看看访问该枚举成员的几种方法:

```
*# set a variable to an enum member*
my_day = Days.MONDAY*# the type of the enum member* **print**(**type**(my_day))*# the enum member as string* **print**(my_day)*# the "name" attribute of the enum member* **print**(my_day.**name**)*# the "value" attribute of the enum member* **print**(my_day.**value**)*# the representational string of the enum member* **print**(**repr**(my_day))Output:
<enum 'Days'>
Days.MONDAY
MONDAY
2
<Days.MONDAY: 2>
```

我们可以在比较和标识测试表达式中使用枚举成员，分别使用相等和标识运算符:

```
*# import the module* **from** enum **import** Enum*# define our enumeration* **class** Weekdays(Enum):
    SUNDAY = 1
    MONDAY = 2
    TUESDAY = 3
    WEDNESDAY = 4
    THURSDAY = 5
    FRIDAY = 6
    SATURDAY = 7**if** Weekdays.SUNDAY != Weekdays.TUESDAY:
    **print**("Sunday and Tuesday are two different days.")
**else**:
    **print**("Sunday and Tuesday are all the same to me.")**if** Weekdays.SUNDAY **is** **not** Weekdays.TUESDAY:
    **print**("Sunday and Tuesday are two different days.")
**else**:
    **print**("Sunday is Tuesday. Don't ask me why.")**if** Weekdays.THURSDAY **is** Weekdays(5):
    **print**("Thursday's assigned number is indeed 5.")
**else**:
    **print**("Wrong number :)")**if** Weekdays(5) == Weekdays(5):
    **print**("Thursday's assigned number is indeed 5.")
**else**:
    **print**("Wrong number :)")Output:
Sunday and Tuesday are two different days.
Sunday and Tuesday are two different days.
Thursday's assigned number is indeed 5.
Thursday's assigned number is indeed 5.
```

## 枚举是不可变的

在 Python 中我们不能对枚举做的事情之一是:我们不能改变它的成员或它们的值。我们也不能在枚举中添加或删除成员。让我们看一些基于我们已经存在的`Weekdays`枚举的例子:

*   试图通过访问其名称(`FRIDAY`)来改变成员的值是不会成功的。一个`TypeError`异常被抛出:

```
*# import the module* **from** enum **import** Enum*# define our enumeration* **class** Weekdays(Enum):
    SUNDAY = 1
    MONDAY = 2
    TUESDAY = 3
    WEDNESDAY = 4
    THURSDAY = 5
    FRIDAY = 6
    SATURDAY = 7*# try to alter Friday's assigned number*
Weekdays["FRIDAY"] = -3Output:
Traceback (most recent call last):
  ...
    Weekdays["FRIDAY"] = -3
TypeError: 'EnumMeta' object does not support item assignment
```

*   试图直接改变成员的值将导致抛出`AttributeError`异常:

```
*# import the module* **from** enum **import** Enum*# define our enumeration* **class** Weekdays(Enum):
    SUNDAY = 1
    MONDAY = 2
    TUESDAY = 3
    WEDNESDAY = 4
    THURSDAY = 5
    FRIDAY = 6
    SATURDAY = 7*# try to alter Saturday's assigned number*
Weekdays.SATURDAY.value = 8Output:
Traceback (most recent call last):
  ...
    Weekdays.SATURDAY.value = 8
  ...
    raise AttributeError("can't set attribute")
AttributeError: can't set attribute
```

*   试图从枚举中删除一个元素会导致另一个`TypeError`异常:

```
*# import the module* **from** enum **import** Enum*# define our enumeration* **class** Weekdays(Enum):
    SUNDAY = 1
    MONDAY = 2
    TUESDAY = 3
    WEDNESDAY = 4
    THURSDAY = 5
    FRIDAY = 6
    SATURDAY = 7*# try to delete MONDAY* **del** Weekdays["MONDAY"]Output:
Traceback (most recent call last):
  ...
    del Weekdays["SATURDAY"]
TypeError: 'EnumMeta' object does not support item deletion
```

*   尝试将成员添加到枚举中(这将引发与我们尝试改变现有成员时相同的错误):

```
*# import the module* **from** enum **import** Enum*# define our enumeration* **class** Weekdays(Enum):
    SUNDAY = 1
    MONDAY = 2
    TUESDAY = 3
    WEDNESDAY = 4
    THURSDAY = 5
    FRIDAY = 6
    SATURDAY = 7*# try to add a member* Weekdays["TEST"] = 16Output:
Traceback (most recent call last):
  ...
    Weekdays["TEST"] = 16
TypeError: 'EnumMeta' object does not support item assignment
```

## 枚举是可哈希的

这对我们来说意味着我们可以使用枚举——以及枚举成员——作为字典中的键，甚至是集合中的元素:

```
*# import the module*
**from** enum **import** Enum**class** Days(Enum):
    SUNDAY = 1
    MONDAY = 2
    TUESDAY = 3
    WEDNESDAY = 4
    THURSDAY = 5
    FRIDAY = 6
    SATURDAY = 7my_dict = {Days: "123", Days(4): "456"}
**print**(my_dict)my_set = {Days, 5, Days(2)}
**print**(my_set)Output:
{<enum 'Days'>: '123', <Days.WEDNESDAY: 4>: 'test_value'}
{<Days.MONDAY: 2>, <enum 'Days'>, 5}
```

可以看出，Python 可以轻松地将枚举成员甚至整个枚举作为集合元素或字典键来处理。

## 填充的枚举是不可继承的

枚举类还有一个特别的地方。我们不能从包含成员的枚举类继承:

```
*# import the module* **from** enum **import** Enum*# define our working days enumeration* **class** WorkWeek(Enum):
    MONDAY = 2
    TUESDAY = 3
    WEDNESDAY = 4
    THURSDAY = 5*# now build a full week class from it* **class** FullWeek(WorkWeek):
    FRIDAY = 6
    SATURDAY = 7
    SUNDAY = 1Output:
Traceback (most recent call last):
  ...
    class FullWeek(WorkWeek):
  ...
    metacls._check_for_existing_members(cls, bases)
  ...
    raise TypeError(
TypeError: FullWeek: cannot extend enumeration 'WorkWeek'
```

但是我们可以从枚举类继承，唯一的条件是它们不包含任何成员:

```
*# import the module* **from** enum **import** Enum*# define our enumeration* **class** Days(Enum):
    **pass***# extend the Days class with some weekend days* **class** Weekend(Days):
    FRIDAY = 6
    SATURDAY = 7
    SUNDAY = 1**for** item **in** Weekend:
    **print**(item)Output:
Weekend.FRIDAY
Weekend.SATURDAY
Weekend.SUNDAY
```

这就是今天关于 Python 枚举的简短文章的内容。你可以在网上找到更多关于它们的信息，还有什么比官方的[文档](https://docs.python.org/3/library/enum.html)更好的起点呢？

保重，身体健康，编码快乐！直到[下一次](https://medium.com/@deck451/python-up-your-code-hashing-b486adc4bf9)！

*Deck 是软件工程师、导师、作家，有时甚至是老师。他拥有 12 年以上的软件工程经验，现在是 Python 编程语言的真正倡导者，同时他的热情是帮助人们提高他们的 Python(以及一般的编程)技能。你可以在* [*【领英】*](https://www.linkedin.com/in/deck451/)*[*【脸书】*](https://www.facebook.com/deck451/)*[*推特*](https://twitter.com/Deck45100)*[*不和谐*](https://discord.com) *: Deck451#6188，以及跟随他写在这里的* [*中*](https://medium.com/@deck451)***