# Python @property 装饰器

> 原文：<https://blog.devgenius.io/python-property-decorator-538beb77cce2?source=collection_archive---------5----------------------->

在 Python 中，装饰器特性包装一个函数，向现有代码添加几个功能，然后返回它。方法和函数是可调用的，因为它们可以被调用。因此，装饰器是一个返回可调用对象的可调用对象。这也称为元编程，因为程序的一部分在编译时改变了程序的另一部分。注意:有关更多信息，请参见 Python 中的 Decorators。

**Python @property decorator**

@property decorator 是一个 Python 内置的 decorator，它允许您定义属性，而不必调用内置函数 property()。它用于从指定的 getter、setter 和 deleter 中返回类的属性特性作为参数。让我们看一些如何在 Python 中使用@property 装饰器的例子:examples:

```
# Python program to illustrate the use of# @property decorator# Defining classclass Portal:# Defining __init__ methoddef __init__(self):self.__name =''# Using @property decorator@property# Getter methoddef name(self):return self.__name# Setter method@name.setterdef name(self, val):self.__name = val# Deleter method@name.deleterdef name(self):del self.__name# Creating objectp = Portal();# Setting namep.name = 'CarToon'# Prints nameprint (p.name)# Deletes namedel p.name# As name is deleted above this# will throw an errorprint (p.name)
```

**输出:**

```
CarToon## An error is thrownTraceback (most recent call last):File "main.py", line 42, inprint (p.name)File "main.py", line 16, in namereturn self.__nameAttributeError: 'Portal' object has no attribute '_Portal__name'
```

这里使用@property decorator 来定义类门户中的属性名，它有三个名称相似的方法(getter、setter 和 deleter)，即 name()，但参数数量不同。name(self)是一个 getter 方法，并标有@property，而 name(self，val)是一个 setter 方法，因为它用于设置属性 __name 的值，并标有@name.setter。deleter 是一个 deleter 方法，可以删除 setter 方法分配的值。但是，使用关键字 del 调用 deleter。范例 2:

```
Python program to illustrate the use of# @property decorator# Creating classclass Celsius:# Defining init method with its parameterdef __init__(self, temp = 0):self._temperature = temp# @property decorator@property# Getter methoddef temp(self):# Prints the assigned temperature valueprint("The value of the temperature is: ")return self._temperature# Setter method@temp.setterdef temp(self, val):# If temperature is less than -273 than a value# error is thrownif val < -273:raise ValueError("It is a value error.")# Prints this if the value of the temperature is setprint("The value of the temperature is set.")self._temperature = val# Creating object for the stated classcel = Celsius();# Setting the temperature valuecel.temp = -270# Prints the temperature that is setprint(cel.temp)# Setting the temperature value to -300# which is not possible so, an error is# throwncel.temp = -300print(cel.temp)
```

**输出:**

```
The value of the temperature is set.The value of the temperature is:-270# An error is thrownTraceback (most recent call last):File "main.py", line 47, incel.temp = -300File "main.py", line 28, in tempraise ValueError("It is a value error.")ValueError: It is a value error.
```

这里抛出一个值错误，因为指定的温度必须大于-273。但是这里零下 300 度。因此，会引发值错误。